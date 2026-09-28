# Unit 1: Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73

**Verdict output**

I first ran the skill live against issue #60 (Faithfulness checker crashes on
`text: None`). It rejected: `unclaimed` failed because PR #80 ("Fixes #60. ...
Now uses `chunk.get("text") or ""`.") was already open against it, a correct
rejection, not a rubric problem, so I moved to a different candidate, #73.

```
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
  "checks": [
    {"name": "maintainer_alive", "grade": "pass", "evidence": "most recent default-branch commit 2f4e82f dated 2026-09-16, within 30 days"},
    {"name": "repo_in_use", "grade": "pass", "evidence": "repo not archived; last push 2026-09-16, within 90 days"},
    {"name": "scope_fits_newcomer", "grade": "pass", "evidence": "bounded doc/config sync: add OPENROUTER_API_KEY to .env.example and align LLM_PROVIDER options; estimated 1-2 hours"},
    {"name": "unclaimed", "grade": "pass", "evidence": "no assignee, no linked or mentioned PR; multiple claim comments exist but scope.md's Path Review house rule exempts claim comments from blocking"},
    {"name": "not_a_graveyard_issue", "grade": "pass", "evidence": "no auto-unassign history, no closed-unmerged PR attempts"},
    {"name": "human_reported", "grade": "pass", "evidence": "opened by Aburke225, not a bot account"},
    {"name": "ai_contribution_allowed", "grade": "pass", "evidence": "no CONTRIBUTING.md or AGENTS.md found in repo root; silence passes"},
    {"name": "good_first_issue_label", "grade": "pass", "evidence": "issue carries the 'good first issue' label"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Only one eval run was performed. From `eval-run.txt`: `agreement: 20/20 scored
items  (bar: 18/20: PASS)`.

**Issue analysis**

`issue-04` (source: `zxcalc/zxlive#555`, "Missing several basic rule previews").
Gold label: `accept`. My rubric's verdict: `accept`, agreement `yes`.

Reasoning: the repo-facts block shows `last 5 default-branch commits` all dated
`2026-08-04`, one day before the `captured: 2026-08-05` date, so `maintainer_alive`
passes on the commit-recency clause alone. `repo_in_use` passes: `archived: no`
and `last push to any branch: 2026-08-04`. `scope_fits_newcomer` passes: the
issue is a single, describable gap ("Including remove identity, fuse spiders,
remove self loops, etc.") rather than an umbrella/tracking issue, and it has
zero comments, which my rubric's evidence-guide-aligned wording treats as "not
evidence of a dispute" rather than a fail. `unclaimed` passes: `this issue:
assignees: none; linked PRs: none`. `not_a_graveyard_issue` and
`human_reported` pass (opened by RazinShaikh, a COLLABORATOR, not a bot).
`ai_contribution_allowed` passes: the contribution policy line states no
statement on AI or contribution tooling, which my rubric treats as silence,
not a ban. Every required check passed, so the verdict rule accepts the issue,
matching the gold label.

**Check rationale**

From `rubric.md`, the `maintainer_alive` row:

> Pass if EITHER at least one of the last 5 default-branch commits is dated
> within 30 days of the capture date (a bot merging a human's PR counts), OR
> at least one entry in the maintainer first-response sample shows a first
> owner/member/collaborator reply within 45 days. A thin or all-"no comment in
> thread" response sample does not fail this check by itself when the
> commit-recency signal already passes

I wrote it as an OR across two independent signals because a maintainer can be
alive by shipping code without triaging issues, or by triaging issues in a
slow-moving repo without pushing commits that week; either alone is real
evidence of a person still present. I added the carve-out clause after
noticing that a maintainer first-response sample can look thin (several
"no maintainer comment in thread" entries) purely because the sampled issues
happen to be recent and unanswered yet, not because the maintainer vanished;
letting that drag down an otherwise-passing commit-recency signal would punish
issues for bad luck in which five issues got sampled.

**Trade-offs**

Issue-04's own first-response sample includes two thin entries, `#512` and
`#553`, both "no maintainer comment in thread", and one very slow one, `#513`
at 92.1 days (outside the 45-day bar). Without the carve-out clause, a strict
reading could treat that response sample as evidence the maintainer is not
actually responsive to issues, even though the commit list shows daily commits
right up to the capture date. The carve-out is exactly what lets
`maintainer_alive` pass on issue-04 through the commit-recency clause alone,
without the thin/slow response sample dragging it down. What it gives up:
a repo that is only shipping automated dependency-bump commits (no human
issue engagement at all) would still pass this check, which is a real gap the
commit-recency-alone path can't distinguish from active human maintenance.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length; a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. This fits my time and interests well: it's a documentation/config edit
   (README.md and .env.example), which matches the low-risk, low-framework-
   overhead first issue I said I wanted in my fit profile, and it's scoped at
   1-2 hours.
2. The verdict correctly identified that the issue is unclaimed in the
   rubric's formal sense (no assignee, no PR), and that it's not blocked by
   the pile of "I'd like to work on this" comments from classmates, that's
   exactly the Path Review house rule at work. What the rubric couldn't
   weigh: how crowded the comment thread already is. A dozen classmates have
   already posted reproduction reports on this exact issue, so even though
   nothing stops me from claiming it, I should expect to be racing several
   other PRs for the same fix.
3. Given how many students are already circling this issue, I expect the
   main difficulty isn't the fix itself (which is small and well-scoped) but
   timing: I'll want to open my PR quickly rather than assuming being
   "unblocked" gives me a lot of runway before someone else merges first.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
