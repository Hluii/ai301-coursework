# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read the repo-facts block first (eval mode) or `scope.md`'s house
   rules plus the repo's CONTRIBUTING/AI-policy docs (live mode). Note
   the stated contribution policy and any AI-use disclosure
   requirement verbatim — `comms_respects_thread_and_repo` needs the
   exact wording, not a paraphrase.
2. Read the issue (title, body, labels). Note the reported behavior in
   one sentence: what should happen vs. what actually happens.
3. Read the thread highlights (eval mode) or the live issue thread
   (live mode). Note any explicit maintainer direction — a stated
   preferred approach, a "please don't," a request for a specific
   kind of fix. An empty highlights block / no maintainer comments
   means there is nothing to check the plan comment against for that
   half of `comms_respects_thread_and_repo`.
4. Read the repro evidence in full, step by step. Note: (a) the
   behavior it confirms, (b) any control or isolation step and what it
   rules in or out — this is the fact `diagnosis_grounded` lives or
   dies on, so write it down before reading the plan, not after.
5. Read the candidate plan (`## Candidate plan` in eval mode;
   `plan.md` in live mode) in full, start to finish, before grading
   anything.
6. Read the candidate plan comment last, immediately before grading
   `comms_respects_thread_and_repo`, since that check is the only one
   it feeds.

## Evidence gathering

- `diagnosis_grounded`: pull the plan's stated cause (its opening
  "Cause:" line or equivalent). Pull every repro-evidence step noted
  in step 4 above, especially any control/isolation result. Record
  whether the cause is consistent with each one.
- `scope_bounded`: pull the plan's "Change:"/scope line and any
  in-scope/out-of-scope statement. Pull the issue's reported behavior
  from step 2. Record the fix site(s) the plan names and whether
  anything beyond them appears in the "Change:" description.
- `plan_executable`: pull the named file(s)/location(s) and the
  approach sentence from the plan's "Change:" section. Record whether
  a single concrete approach is stated or whether a choice is left
  open.
- `test_plan_decisive`: pull the plan's "Test:" line. Record whether it
  names a specific repro step/artifact and a specific expected change,
  or only a general standard.
- `comms_respects_thread_and_repo`: pull the full plan comment text.
  Record, side by side, (a) whether it matches or contradicts the
  maintainer direction noted in step 3 — if step 3 recorded no
  direction, this half passes with no further check — and (b) whether
  it discloses AI assistance if the policy noted in step 1 requires it
  — if step 1 recorded no such requirement, this half passes with no
  further check. Grade `pass` unless an actual contradiction or an
  actual missing required disclosure was recorded; never grade this
  check `unclear`.
- `honesty_of_risks`: pull any risks/unknowns line from the plan.
  Record whether it names a real unverified unknown or restates
  confidence instead.

## Check execution

Grade the required checks in this order, since each later check
assumes the fact the earlier one recorded: `diagnosis_grounded` →
`scope_bounded` → `plan_executable` → `test_plan_decisive` →
`comms_respects_thread_and_repo`. Grade `honesty_of_risks` last; it
never changes the verdict, so it never blocks the others.

For each check, apply its rubric pass condition to the evidence
recorded above — do not re-read the whole package again unless a
check's evidence wasn't captured during the read-order pass. If the
evidence a check needs is genuinely absent from the package (e.g. no
risks line at all, no thread highlights and no stated AI policy),
grade `unclear` with the evidence field stating what is missing; do
not infer or assume evidence that was not written down.

A later check is graded independently of an earlier check's result:
a plan that already failed `diagnosis_grounded` still gets
`scope_bounded` through `honesty_of_risks` graded on their own
evidence, so the output shows every reason the plan isn't ready, not
just the first one found.

## Verdict assembly

Apply the rubric's verdict rule: `accept` only if all five required
checks (`diagnosis_grounded`, `scope_bounded`, `plan_executable`,
`test_plan_decisive`, `comms_respects_thread_and_repo`) grade `pass`.
Any required check graded `fail` or `unclear` makes the verdict
`reject`. `honesty_of_risks` is reported in the checks list but never
changes `accept`/`reject`.

In the output JSON, the `evidence` field for whichever required check
failed (or, if several failed, the first one in execution order) is
what gets quoted back as the deciding reason; if all required checks
pass, quote the `test_plan_decisive` evidence as the deciding reason
the plan is ready to build from.
