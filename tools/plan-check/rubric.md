# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis_grounded | The plan's stated cause, read against every step and any control/isolation result in the repro-evidence block (see `references/evidence-guide.md`'s Diagnosis and grounding section) | Pass if the stated cause is consistent with every observation the repro evidence contains, and is not ruled out by a control or isolation step it includes (e.g. a step that reproduces with one variable held constant, or a step that shows the "blamed" code path wasn't exercised). Fail if any repro-evidence step contradicts or rules out the stated cause, even when the rest of the plan reads as confident and polished. Unclear if the plan states no cause at all. | Required |
| scope_bounded | The plan's described change and any stated out-of-scope line, read against what the issue's reported behavior actually requires (see Evidence guide's Scope section) | Pass if the described change is limited to the fix site(s) the issue's behavior requires, with any adjacent work named and excluded rather than folded in. Fail if the plan bundles the fix with an unrelated refactor, migration, redesign, new option, or module restructure that the issue did not ask for, even when the core fix inside it is correct. | Required |
| plan_executable | The plan's named files/areas and chosen approach (see Evidence guide's Executability section) | Pass if specific file(s) or location(s) and one concrete, chosen approach are named, such that a stranger could start making the change without asking the author a clarifying question. Fail if the approach is still a choice to be made ("investigate the input stack", "fix upstream or vendored, whichever is easier"), or no file/location is named. | Required |
| test_plan_decisive | The plan's test plan, read against the repro evidence's steps and artifacts (see Evidence guide's Test plan section) | Pass if the test plan names a specific, observable outcome tied to the repro evidence (e.g. "re-run step 3; the color must flip without leaving the view"). Fail if the test plan names no observable outcome for the fix itself ("should feel fast", "run the full test suite", "nothing else should feel broken"). | Required |
| comms_respects_thread_and_repo | The candidate plan comment, read against the thread highlights for explicit maintainer direction and against the repo-facts block's stated AI-use disclosure requirement (see Evidence guide's Comms section) | Pass by default. Fail only if the comment contradicts or ignores explicit maintainer direction that is actually present in the thread highlights, or if the repo-facts block states the repo requires disclosing AI assistance and the comment discloses none. When the thread highlights are empty, there is no direction to contradict, so that half of the check passes vacuously; when the repo-facts block states no AI policy (or none at all), there is nothing to disclose, so that half passes vacuously too. Never grade this check `unclear`: absence of a requirement is a pass, not an open question. | Required |
| honesty_of_risks | The plan's risks/unknowns statement, read against the confidence of its diagnosis and scope (see Evidence guide's Honesty section) | Pass if genuine unknowns (untested platforms, deferred edge cases, assumptions the author couldn't verify) are named as such. Fail if the plan states no risks at all while resting on an assumption the repro evidence doesn't fully confirm. | Preferred |

## Verdict rule

Accept if every `required` check grades `pass`. Any `required` check graded `fail` or `unclear` results in `reject`: a plan whose grounding, scope, executability, test plan, or thread/repo compliance cannot be verified from the package is not ready to post and build from. The `preferred` check (`honesty_of_risks`) is reported but never changes the verdict.
