# Procedure: how this tool grades a PR package

## Read order

Read the plan first, then the issue it belongs to, then the candidate
PR's description, then its diff, then its test evidence last. The plan
goes first because every side-by-side check in this rubric measures the
PR against the plan's stated scope and test plan; reading the diff
before the plan invites judging the diff on its own merits instead of
against what was promised. Note while reading the plan: its stated
files, its stated boundary (what it explicitly rules out), its test
plan, and any `## Deviations` entry. Note while reading the issue: the
original reproduction's observable behavior, since the test evidence
must resolve that same behavior, not an adjacent one. Note while reading
the description: every factual claim about what changed and what was
tested, to check against the diff and test evidence next. Read the diff
file-by-file against the plan's stated file list. Read the test evidence
last, against both the plan's test plan and the issue's repro steps.

## Evidence gathering

- **matches_plan_scope**: list every file the diff touches. For each,
  check it against the plan's stated file list. A file not named in the
  plan needs a deviation note in plan.md that says what changed and why;
  without one, record the file as out-of-scope evidence.
- **description_matches_diff**: list every claim the description's
  Summary and Changes sections make. For each, find the diff hunk that
  supports it, or record its absence. Separately, scan the diff for any
  change with no corresponding claim in the description.
- **test_evidence_decisive**: pull the issue's original repro steps and
  the plan's test plan. Pull the PR's before/after output. Record
  whether the "before" reproduces the same observable condition the
  issue reports, and whether the "after" shows it resolved.
- **repo_checks_run**: list every check the repo's PR template or
  CONTRIBUTING.md names as required (lint, typecheck, unit tests,
  integration tests, etc). For each, find its pasted output in the
  Testing section, including an honest "no tests ran" or a failure.
- **diff_is_reviewable**: scan the diff for lines outside the hunks that
  implement the plan's change: commented-out code, debug prints, pure
  formatting churn, or files/hunks unrelated to the plan's stated scope.
- **template_sections_filled**: list every section the repo's PR
  template defines. For each, check the candidate description has real
  content there, not the template's own placeholder text left in place.
- **ai_disclosure_present**: find the repo's stated AI-use policy (the
  repo-facts block live, or CONTRIBUTING.md/AI_USAGE_POLICY.md on
  GitHub). Find the description's AI-use disclosure, wherever the
  template places it or, if the template has no such section, under
  Notes for Reviewers.
- **issue_referenced**: read the description's Issue section for a
  concrete issue number.

## Check execution

Execute the checks in the table order given in `rubric.md`. For each,
apply its pass condition to the evidence gathered above and grade
`pass`, `fail`, or `unclear`, writing the one fact or quote that decided
it. Grade a check `unclear` only when the evidence it needs is genuinely
absent from the package (eval mode) or unobtainable after a real attempt
to gather it (live mode), never because the evidence is inconvenient to
read. A check may be graded without re-reading the whole package once
its evidence has been noted during the read-order pass; do not re-derive
evidence already recorded.

## Verdict assembly

Apply the rubric's verdict rule: accept only if every required check
passed; any required check graded `fail` or `unclear` rejects the
package. When more than one required check failed, quote the first
failing required check in the rubric's table order as the deciding
check in the output summary. Preferred checks are reported but never
change the verdict. State the verdict and emit the fenced JSON block
exactly as `SKILL.md` and `CONTRACT.md` specify.
