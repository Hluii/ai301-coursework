# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Hluii

---

## Posted upstream

**Claim comment**

[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-5905782125]


Hello, I'm new to this repo and would like to take on this issue. I will compare the README.md instructions for OPENROUTER_API_KEY with .env.example, then go through the setup to find mismatches in the required configuration documentation.

**Reproduction comment**

[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-5906007459]

**Environment:** Ubuntu 24.04.2 LTS, Python 3.12.3, commit `2f4e82f` (2026-09-16). The OS and Python version don't matter here since it is a mismatch between the docs and the config template.
 
**Steps:**
 
1. Follow the README.md Quick Start comment `# Configure environment (add your OPENROUTER_API_KEY to .env)` and the docs/SETUP.md instruction "Edit .env and set your OPENROUTER_API_KEY (required for AI features)". Both docs tell you to run this first:
```
   cp .env.example .env
```
2. Search the new `.env` for the variable the docs told you to set:
```
   $ grep -n "OPENROUTER_API_KEY" .env
   (no output)
```
3. Check the LLM provider block in `.env.example`:
```
   # LLM provider
   # Options: "mock" (default, no API key needed), "openai"
   LLM_PROVIDER=mock
   OPENAI_API_KEY=sk-your-key-here
```
   The comment lists only `mock` and `openai`, and only `OPENAI_API_KEY` is present. There is no `OPENROUTER_API_KEY`, `OPENROUTER_BASE_URL`, or `OPENROUTER_MODEL`.
4. Compare this with `core/config.py`, which defines all three as settings fields:
```python
   llm_provider: str = Field(default="mock")
   openai_api_key: str = Field(default="")
   openrouter_api_key: str = Field(default="")
   openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
   openrouter_model: str = Field(default="google/gemma-3-27b-it:free")
```
 
**Expected:** After copying `.env.example` to `.env`, there should be an `OPENROUTER_API_KEY` line to fill in, and the `LLM_PROVIDER` comment should list every provider the app supports.
 
**Actual:** Neither is true. Someone following README.md and SETUP.md has no place to put the key the docs tell them to set, and the `LLM_PROVIDER` comment leaves out `openrouter` even though `core/config.py` supports it.
 
**Result: reproduced.** The three files disagree as described. I didn't run the app with `LLM_PROVIDER=openrouter`, since this is a static file mismatch and doesn't need a live run.
 
I'll open a PR that adds the three OpenRouter variables to `.env.example`, adds `openrouter` to the `LLM_PROVIDER` comment, and updates the README.md Quick Start comment so all three files agree.
 


## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One full run: `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with every category
matched (`clear-accept 7/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3
wrong-target 4/4`). This is the run committed in `eval-run.txt`. It followed two rubric
revisions made before this run: an earlier draft rubric had no check reading the claim
comment against the repo's own AI-disclosure policy, and its `behavior_match_issue` check
failed any package whose excerpt showed "no failure at all" — which would have wrongly
rejected an honest cannot-reproduce report. Both were fixed before this confirming run, so
only one full run was needed after the fixes.

**Package analysis**

`pkg-20` (ghostty-org/ghostty#13604): gold label is `reject`, and my rubric agrees. The repro
itself is flawless on every proof check (environment, followable steps, a matching artifact,
an honest stated outcome), but Ghostty's `AI_POLICY.md` requires disclosing all AI usage,
naming the tool and the extent of assistance, and neither the claim comment nor the repro
report discloses anything. My rubric's `comms_respects_repo` check reads the repo-facts
block's stated AI policy against the comment text specifically for this, and fails the
package on that missing disclosure alone, regardless of how strong the repro evidence is.
That is the intended read: disclosure is a wall, not a quality signal that trades off against
the rest of the package.

**Check rationale**

`comms_respects_repo | The claim comment and repro report text, read against the repo-facts
block's contribution policy and any stated AI-use disclosure requirement (see
references/evidence-guide.md's Comms section) | Pass if the claim comment states specific,
honest intent in the author's own words and, when the repo's policy requires disclosing AI
assistance, that disclosure is present. Fail if the comment is generic boilerplate or
over-promising with no real intent, or if a stated disclosure requirement is not met. |
Required`

My first draft of the rubric had no required check that ever read the claim comment's
content: the only comms-facing check was `words_respected`, which is `Preferred` and only
reads `voice-guide.md`, a file the eval harness ignores entirely. That meant nothing in eval
mode could ever catch a disclosure violation or a boilerplate claim comment, so `pkg-20`
(disclosure) and `pkg-19` (boilerplate "I'll fix it in 2 days guaranteed" claim) both
disagreed with gold on my first run. I rejected keeping comms as preferred-only and added
`comms_respects_repo` as a required check pointed at the repo-facts block instead of the
voice guide, since repo policy compliance is a gate every stranger's package has to clear,
not a personal-style preference.

**Trade-offs**

Fixing `comms_respects_repo` meant also fixing `behavior_match_issue`, and that check gives up
strictness on purpose: its pass condition now treats a "no failure at all" excerpt as
expected, not disqualifying, whenever the report's claimed outcome is cannot-reproduce. Before
that change, `pkg-09` and `pkg-10` (both gold `accept`, both honest cannot-reproduce reports
whose whole point is a clean run) failed `behavior_match_issue` and were wrongly rejected. The
trade-off: `behavior_match_issue` alone can no longer tell a real "gave it a real, thorough
attempt" cannot-reproduce from a "barely tried, pasted a trivial clean run and called it done"
one; the carve-out only checks that the excerpt is consistent with the claimed outcome, not
how hard the attempt was. That distinction is left to `steps_complete_and_followable` (did the
attempt actually exercise the issue's trigger conditions?) rather than caught here. I accept
this as a case the check can miss: a thin but technically-consistent cannot-reproduce report
could still pass `behavior_match_issue` on the strength of the carve-out alone. The one
disagreement in my confirming run, `pkg-01` (gold `accept`, mine `reject`, failing
`environment_recorded`), is unrelated to this change; the report's stated environment reads as
consistent with the issue to me on a manual check, so I'm treating that one as grader
variance rather than a rubric wording problem, since it's the only miss and the run is still
over the bar.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
