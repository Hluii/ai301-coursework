# Plan - issue #73: OpenRouter env vars missing from `.env.example`

## Diagnosis

README.md's Quick Start and `docs/SETUP.md` both tell a new contributor to set
`OPENROUTER_API_KEY` in `.env` right after `cp .env.example .env`, but
`.env.example`'s LLM provider block has no `OPENROUTER_API_KEY` line, no
`OPENROUTER_BASE_URL`, and no `OPENROUTER_MODEL`, and its `# Options:` comment
lists only `"mock"` and `"openai"`. `core/config.py` defines all three
OpenRouter fields as real settings
(`openrouter_api_key`, `openrouter_base_url`, `openrouter_model`, alongside
`llm_provider`), so the app supports an `openrouter` provider that neither
doc-facing file mentions.This is a three-way disagreement between the two
docs and the config template. I confirmed in my unit-2
repro by diffing `.env.example` against `core/config.py` field-by-field.

> Quoted from my repro comment (issue #73):
> "The comment lists only `mock` and `openai`, and only `OPENAI_API_KEY` is
> present. There is no `OPENROUTER_API_KEY`, `OPENROUTER_BASE_URL`, or
> `OPENROUTER_MODEL`... core/config.py, which defines all three as settings
> fields: `llm_provider`, `openai_api_key`, `openrouter_api_key`,
> `openrouter_base_url`, `openrouter_model`."

## Scope

**In scope:** bringing `.env.example`, `README.md`, and `docs/SETUP.md` into
agreement with what `core/config.py` already supports, for the OpenRouter
provider only.

**Not in scope:**
- Any change to `core/config.py` or to how a provider is selected/dispatched
  at runtime (I checked: no code currently branches on `settings.llm_provider`
  at all; that's a separate, pre-existing gap outside this issue, which is
  purely a documentation/template mismatch).
- Adding support for any provider beyond the three `core/config.py` already
  defines (`mock`, `openai`, `openrouter`).
- Any other documentation issue not touching these three files.

## Files I'll touch

- `.env.example`: add `OPENROUTER_API_KEY`, `OPENROUTER_BASE_URL`,
  `OPENROUTER_MODEL` to the LLM provider block, and add `"openrouter"` to the
  `# Options:` comment.
- `README.md`: the Quick Start comment `# Configure environment (add your
  OPENROUTER_API_KEY to .env)` currently implies the key already has a place
  in `.env.example`; no wording change needed there once `.env.example` is
  fixed, but I'll re-check it reads correctly once the variable exists.
- `docs/SETUP.md`: same check: `# Edit .env and set your OPENROUTER_API_KEY
  (required for AI features)` currently points at a key that doesn't exist in
  the file it tells you to copy; same re-check after `.env.example` is fixed.

## Approach

1. Edit `.env.example`'s LLM provider block:
   - Change `# Options: "mock" (default, no API key needed), "openai"` to
     `# Options: "mock" (default, no API key needed), "openai", "openrouter"`.
   - Add three lines after `OPENAI_API_KEY=sk-your-key-here`:
     `OPENROUTER_API_KEY=sk-or-your-key-here`,
     `OPENROUTER_BASE_URL=https://openrouter.ai/api/v1`,
     `OPENROUTER_MODEL=google/gemma-3-27b-it:free`, matching `core/config.py`'s
     field defaults exactly, so a fresh `.env`
     from the template already has the right default base URL and model.
2. Re-read README.md's Quick Start block and `docs/SETUP.md`'s step 2 against
   the corrected `.env.example`, confirming both now point at a key that
   actually exists in the file they tell the reader to copy. No wording
   change is needed in either file unless this re-check turns up something
   the diagnosis missed.
3. Run `make lint && make typecheck`. These are plain-text/env file changes
   so neither should be affected, but CONTRIBUTING.md requires CI green on
   every PR.

## Test plan

Re-run my unit-2 repro steps against the fix:

1. `cp .env.example .env` (fresh copy, post-fix).
2. `grep -n "OPENROUTER_API_KEY" .env`. Before: `(no output)`. Expected
   after: one line, `OPENROUTER_API_KEY=sk-or-your-key-here`.
3. Re-inspect the LLM provider block in `.env`. Before: comment lists only
   `mock`/`openai`, only `OPENAI_API_KEY` present. Expected after: comment
   lists `mock`, `openai`, `openrouter`; all three OpenRouter variables
   present with the same defaults as `core/config.py`.
4. Re-read README.md Quick Start and `docs/SETUP.md` step 2. Expected after:
   both instructions now name a variable that exists in the file they tell
   the reader to create.

## Risks and unknowns

- I'm assuming the placeholder value style (`sk-or-your-key-here`) should
  mirror `.env.example`'s existing `OPENAI_API_KEY=sk-your-key-here`
  placeholder convention; I haven't found a repo style rule that specifies
  this, so I'm matching the nearest existing example.
- `core/config.py`'s `openrouter_base_url` and `openrouter_model` already have
  working defaults, so I'm shipping those same values uncommented in
  `.env.example` (as real defaults, not placeholders) rather than as
  commented-out examples. This matches how `DATABASE_URL` and `REDIS_URL`
  are already presented as live defaults in the same file, but it's a
  judgment call since those two don't have placeholder keys attached.
- I confirmed no code currently dispatches on `settings.llm_provider`, so this
  fix is docs/template-only; if that turns out to be wrong (some dispatch
  logic I missed), the scope would need to grow, which I'm treating as
  unlikely given the grep came back empty across the whole codebase.

## Deviations

Nothing changed in the diagnosis, scope, or files touched. One small deviation 
in the approach: I skipped `make lint && make typecheck`
(step 3 of Approach) because no local venv was set up and the only change is
to `.env.example`, a non-Python text file that neither `ruff` nor `mypy`
touches. README.md and `docs/SETUP.md` needed no wording changes, as the
plan already anticipated. Re-reading both after the `.env.example` fix
confirmed they now point at a variable that actually exists in the file they
tell the reader to copy.
