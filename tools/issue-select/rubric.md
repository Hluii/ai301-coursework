# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer_alive | "last 5 default-branch commits" and "maintainer first-response sample" in the repo-facts block (or the equivalent commit list and issue-response latency on github.com) | Pass if EITHER at least one of the last 5 default-branch commits is dated within 30 days of the capture date (a bot merging a human's PR counts), OR at least one entry in the maintainer first-response sample shows a first owner/member/collaborator reply within 45 days. A thin or all-"no comment in thread" response sample does not fail this check by itself when the commit-recency signal already passes | required |
| repo_in_use | "last push to any branch" and "archived:" in the repo-facts block | The repo is not archived, AND the last push is dated within 90 days of the capture date | required |
| scope_fits_newcomer | the issue body and the full comment thread | Pass unless the issue is an explicit umbrella/tracking list of sub-items, is a pure usage/support question with no proposed change, or the thread shows an unresolved, multi-stakeholder design debate with no settled spec (years of "what should this even do" back-and-forth, no maintainer sign-off on a final shape). A single concrete, describable bug or narrowly stated feature ask is NOT failed just because it has zero comments or no maintainer triage yet — silence is not evidence of a dispute | required |
| unclaimed | "this issue: assignees" and "linked PRs" in the repo-facts block, PLUS any PR or "I'll take this / working on this" claim mentioned anywhere in the comment thread (not only formally linked PRs) | No assignee is set, no linked PR is open, and no comment thread claim or referenced PR (formally linked or merely mentioned) is active and unresolved | required |
| not_a_graveyard_issue | the full comment thread, looking for bot messages unassigning a contributor for inactivity, and the "linked PRs" line in the repo-facts block | Fail if the thread shows 3 or more distinct contributors claimed-then-auto-unassigned for inactivity, OR 2 or more linked PRs attempting this issue were opened and later closed without merging. An issue with this history keeps eating contributors even when it looks unclaimed right now | required |
| human_reported | the issue's "opened by <user> (<ROLE>)" line | Fail if the opening account is a bot (username ends in `[bot]`, or its role/type is otherwise marked as a bot/app account). An issue filed by an automated tool with no human maintainer triage behind it is not a vetted ask | required |
| ai_contribution_allowed | the "contribution policy" line in the repo-facts block (CONTRIBUTING.md / AI_USAGE_POLICY.md / AGENTS.md) | The policy does not state an outright ban on AI-generated contributions. Conditions (disclosure, human review, understanding every change) pass; silence (no policy stated) passes | required |
| good_first_issue_label | the issue's labels line | The issue carries a "good first issue" label (or repo equivalent) | preferred |

## Verdict rule

Accept if every required check (`maintainer_alive`, `repo_in_use`,
`scope_fits_newcomer`, `unclaimed`, `not_a_graveyard_issue`,
`human_reported`, `ai_contribution_allowed`) grades `pass`. Any required
check graded `fail` or `unclear` rejects the issue —
`unclear` is treated as `fail` throughout, since a first issue you cannot
verify is not one to take. `good_first_issue_label` is preferred: it never
changes the verdict, it only ranks accepted issues (labeled ones rank
above unlabeled ones).