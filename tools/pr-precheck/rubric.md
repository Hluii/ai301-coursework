# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| matches_plan_scope | the diff's changed files and hunks, read against the plan's stated files and boundary (and any `## Deviations` note in plan.md) | Every changed file and every hunk falls inside the plan's stated scope, or is covered by an explicit deviation note that says what changed and why. A file or hunk with no plan coverage and no deviation note fails this check, in either direction (the diff does more than the plan, or quietly does less by abandoning a stated part with no note) | required |
| description_matches_diff | the PR description's claims (Summary and Changes sections) read against the actual diff content | Every claim the description makes about what changed is observable in the diff, and the diff introduces nothing the description is silent about. A description claiming "implements the plan exactly" or "no functional changes outside X" while the diff shows otherwise fails this check | required |
| test_evidence_decisive | the Testing section's before/after output, read against the plan's test plan and the issue's original reproduction steps | The before output reproduces the issue's stated behavior, and the after output shows that behavior resolved, using the same observable condition (not a different, adjacent one). "Tests pass" or "works now" with no named before/after state does not satisfy this; an honestly reported cannot-fully-verify (e.g. a skipped integration suite with a stated reason) does not fail this check by itself | required |
| repo_checks_run | the Testing section's record of the repo's own named check commands (the test suites, linter, typechecker, and any codegen/doc-sync command the repo's CONTRIBUTING.md or PR template names as something to run, such as `make lint`, `cargo test`, `go generate ./...`) | Every check command the repo explicitly names was actually run, with a concrete, named result for each: which command, and its specific outcome (a pass/fail state, a test count, a named diff/failure), even when a check fails or reports "no tests ran". Raw pasted terminal output satisfies this but is not required. A vague "tests pass" or "all checks green" with no command named is not enough, and a check silently skipped fails this. A template requirement that is a property of the code rather than a command to run (for example, a formatting convention with no stated command, or an i18n string the diff itself already shows was added) is satisfied by the diff/commit evidence itself and does not need a separate named run | required |
| diff_is_reviewable | the unified diff and the commit list | The diff contains only the change the plan scopes (per matches_plan_scope), with no debris: no commented-out code, no debug prints or leftover logging, no formatting-only churn mixed into functional hunks, no unrelated drive-by edits riding along | required |
| template_sections_filled | the repo's PR template sections (from the repo-facts block or `.github/PULL_REQUEST_TEMPLATE.md`), read against the candidate description | Every section the repo's template requires carries real, specific content, not boilerplate or a placeholder left in place. A section that does not apply states why instead of being left blank | required |
| ai_disclosure_present | the repo's stated AI-use/contribution policy, read against the description's Notes for Reviewers (or equivalent) section | If the repo's policy asks for AI-use disclosure, the description states concretely how AI was used on the change. If the repo states no such policy, this check passes regardless (silence is not a ban, and nothing is owed beyond general honesty) | required |
| issue_referenced | the description's Issue section | The description names the specific issue number the plan belongs to (a "Closes #N" or equivalent reference), not just a repo link or no reference at all | required |
| commit_history_clean | the commit list | Preferred only: commits are scoped and individually readable (no "wip", no "fix typo" noise commits layered on top of the real change). Never changes the verdict | preferred |

## Verdict rule

Accept if every required check (`matches_plan_scope`,
`description_matches_diff`, `test_evidence_decisive`,
`repo_checks_run`, `diff_is_reviewable`, `template_sections_filled`,
`ai_disclosure_present`, `issue_referenced`) grades `pass`. Any required
check graded `fail` or `unclear` rejects the package; `unclear` is
treated as `fail` throughout, since a PR whose claim cannot be verified
from the package is not one that is ready to submit. `commit_history_clean`
is preferred: it never changes the verdict, and only appears in the
summary as a note.

An honestly disclosed shortfall does not, by itself, fail
`test_evidence_decisive` or `template_sections_filled`: a PR that says
plainly "the integration suite has no tests to run, as the repo's
template expects" or "I did not cover the mobile layout; noted under
Deviations" can still pass those checks. What fails them is a claim of
completeness the evidence contradicts, or a gap left unacknowledged.
