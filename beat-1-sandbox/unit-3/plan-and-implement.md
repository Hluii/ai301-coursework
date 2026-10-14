# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Hluii

**Plan comment**

[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-6031291572](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-6031291572)

Plan for the fix, building on my repro above: this is a three-way mismatch
between `.env.example`, README.md's Quick Start, and `docs/SETUP.md`, not a
code defect. `core/config.py` already defines `openrouter_api_key`,
`openrouter_base_url`, and `openrouter_model` as real settings, but
`.env.example` never surfaces them and its `# Options:` comment only lists
`mock`/`openai`.

Change: add `OPENROUTER_API_KEY`, `OPENROUTER_BASE_URL`, and
`OPENROUTER_MODEL` to `.env.example`'s LLM provider block (matching
`core/config.py`'s own defaults for the base URL and model), and add
`"openrouter"` to the `# Options:` comment. I'll then re-check README.md's
Quick Start line and `docs/SETUP.md`'s step 2 now that the variable they tell
readers to set actually exists in the file they tell readers to copy.

Not touching `core/config.py` or any provider-dispatch logic. I checked and
nothing in the codebase currently branches on `settings.llm_provider`, so
that's a separate gap outside this issue's docs-mismatch scope.

Test: re-run my repro steps against the fix. `cp .env.example .env`, then
`grep -n "OPENROUTER_API_KEY" .env` should return a line instead of nothing,
and the LLM provider comment should list all three options.

I'll send a PR shortly.

---

## Your branch

**Branch**

fix/73-openrouter-env-docs

**Evidence**

Before (on `main`, pre-fix):

```
$ cp .env.example .env
$ grep -n "OPENROUTER_API_KEY" .env
(no output)

$ sed -n '/# LLM provider/,/^$/p' .env
# LLM provider
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here
```

After (on `fix/73-openrouter-env-docs`, post-fix):

```
$ cp .env.example .env
$ grep -n "OPENROUTER_API_KEY" .env
20:OPENROUTER_API_KEY=sk-or-your-key-here

$ sed -n '/# LLM provider/,/^$/p' .env
# LLM provider
# Options: "mock" (default, no API key needed), "openai", "openrouter"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here
OPENROUTER_API_KEY=sk-or-your-key-here
OPENROUTER_BASE_URL=https://openrouter.ai/api/v1
OPENROUTER_MODEL=google/gemma-3-27b-it:free
```

The key my unit-2 repro showed was missing (`OPENROUTER_API_KEY`) is now present, along with
`OPENROUTER_BASE_URL` and `OPENROUTER_MODEL`, and the `# Options:` comment now lists
`openrouter` alongside `mock` and `openai` — matching the three fields `core/config.py`
already defined. README.md's Quick Start line and `docs/SETUP.md`'s step 2 were re-read
against this fix and needed no wording change: both already named `OPENROUTER_API_KEY`,
which now exists in the file they tell the reader to copy.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Three runs: a `--limit 4` smoke run scored `3/4`, failing `pkg-02` (gold `accept`) on
`comms_respects_thread_and_repo`; a `--only pkg-01,pkg-02,pkg-03,pkg-04,pkg-20` re-grade after
fixing that check scored `5/5` (all five, including the two `thread-convention` canaries,
agreed); and the confirming full run scored `agreement: 20/20 scored items (bar: 18/20:
PASS)`, with every category matched (`clear-accept 7/7 scope-creep 4/4 thread-convention 2/2
unbuildable 3/3 wrong-cause 4/4`). This last score matches the `eval-run.txt` committed in
this directory.

**Package analysis**

`pkg-02` (sharkdp/bat#3844): gold label is `accept`, and my rubric now agrees — but my first
smoke run graded it `reject`. The plan itself is strong (a grounded `saturating_sub` fix at
the exact underflow site the issue and repro evidence pin down, bounded scope, a decisive
repro-based test), and the package has 0 thread comments and a repo-facts block stating "no
stated AI policy." My first-draft `comms_respects_thread_and_repo` check graded that
combination `unclear` ("nothing to check against"), and my verdict rule treats `unclear` as
`fail`, so a perfectly good plan was rejected for a check that had nothing to hold it to. I
rewrote the check so absence of a requirement is a `pass`, not an open question — see Check
rationale below — and the re-graded package now agrees with gold.

**Check rationale**

`comms_respects_thread_and_repo | The candidate plan comment, read against the thread
highlights for explicit maintainer direction and against the repo-facts block's stated
AI-use disclosure requirement (see Evidence guide's Comms section) | Pass by default. Fail
only if the comment contradicts or ignores explicit maintainer direction that is actually
present in the thread highlights, or if the repo-facts block states the repo requires
disclosing AI assistance and the comment discloses none. When the thread highlights are
empty, there is no direction to contradict, so that half of the check passes vacuously; when
the repo-facts block states no AI policy (or none at all), there is nothing to disclose, so
that half passes vacuously too. Never grade this check `unclear`: absence of a requirement is
a pass, not an open question. | Required`

My first draft's pass condition read almost the same, but with an `Unclear if the thread
highlights are empty and the repo-facts block states no AI policy (nothing to check
against)` clause instead of a vacuous pass. That clause is what sank `pkg-02` and
`calib-04`-adjacent cases: an eval package with nothing stated on either side of the check is
the common case (most of these repos have no AI policy and the captured issue has no
comments), not a rare edge case, so grading it `unclear` — which my verdict rule treats as
`fail` — turned the most frequent package shape into an automatic reject regardless of plan
quality. I rejected keeping `unclear` for "nothing stated" and rewrote the check so absence of
a requirement passes outright, reserving `fail` for an actual, evidenced contradiction or
omission, which is what the `thread-convention` category (`pkg-04`, `pkg-20`) is actually
built to test.

**Trade-offs**

Loosening `comms_respects_thread_and_repo` to pass by default gives up the ability to ever
flag a plan comment as poor communication when the thread is silent and the repo states no
policy — a boilerplate, generic, or tone-deaf comment in that situation now sails through this
check untouched, even though a stricter reading might want to penalize it. I accept that gap:
the check's job, per its evidence (thread highlights and the repo-facts AI-policy line), is to
catch contradiction and omission against stated facts, not general comment quality, and a
separate `preferred` check (`honesty_of_risks`) already covers one axis of write-up honesty.
After loosening it, I re-ran canaries from both affected categories before the confirming full
run — `pkg-04` and `pkg-20` (the two `thread-convention` packages, both gold `reject`, both
already agreeing) stayed `reject` in the `--only` re-grade, confirming the loosened check still
fails a real contradiction/omission and the change only fixed the vacuous-case false reject.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
