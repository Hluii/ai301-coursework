---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: is this
pull request ready to submit? A PR package is a candidate pull request,
title, description, commits, and diff, plus its test evidence, read
against the plan it claims to implement and the issue that plan belongs
to. You never answer a different question, never grade more than one
package per run, and never answer from gut feel: you execute the checks
in `rubric.md`, in the order `procedure.md` states, against evidence you
gather yourself.

## Inputs and modes

One of:

- **Live mode**: the student's own submission, checked before it goes
  out. Inputs: their `plan.md` (read its `## Deviations` section too),
  the diff on their branch (everything the branch changes relative to
  the repo's default branch: `git diff main...HEAD`, three dots, run
  from the working copy, is the command that produces it), their draft
  PR title and description, and their test evidence, read against their
  issue. Gather issue-side evidence live: the issue thread, the repo's
  PR template, and `docs/CONTRIBUTING.md`, via `gh`, the GitHub API, or
  the web. A house-chain student reads the house plan and the house
  repro pack in place of their own; grade the same checks against those.
- **Eval mode**: a package bundle is the whole world. Every fact comes
  from the bundle text (the repo-facts block, the plan context, and the
  candidate PR's title, description, commits, diff, and test evidence).
  Do not fetch anything. Eval mode always grades a complete package:
  every check, the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It names the repo your pull request must target and the Path
Review house rules (PR from your own fork's branch, one PR per issue
per student, the template is always used, a classmate's PR on the same
issue never blocks yours). Refuse to grade a PR package aimed at any
other repository. If the scope's `Repo:` line still carries its
bracketed placeholder, stop without grading and tell the student to
fill it in before running live mode again. In eval mode, ignore
`scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream. Hold the draft PR title and description
against those rules and report any rule they break in the summary,
quoting the rule. The voice guide never changes the verdict on its own,
only a rubric check reading it can do that. In eval mode, ignore
`voice-guide.md` entirely: voice is personal and carries no gold
labels; universal communication-quality checks live in the rubric.

## Component reads

`rubric.md` defines the checks (name, evidence, pass condition, weight)
and the verdict rule that combines their grades into `accept` or
`reject`. `references/evidence-guide.md` maps where each evidence
family lives in a package, in eval bundles and in live mode. Read both
before grading anything. `procedure.md` is the operating sequence:
execute it as written, in the order it states, for read order, evidence
gathering, check execution, and verdict assembly. If a step in
`procedure.md` is silent on something a check needs, report the gap in
the summary; never invent a step to fill it. If `rubric.md` or
`procedure.md` has no content filled in, stop and say so: this skill
cannot grade without them, and that is by design, since the judgment
is the student's own.

## Verdict and output

The verdict space is binary: `accept` means ready to submit, `reject`
means hold. There is no third verdict and no score; reservations belong
in a check's evidence line, not in the verdict. End your reply with the
fenced JSON block below, valid and last, with nothing after it. The
schema is fixed and reproduced from `CONTRACT.md` verbatim:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

A short readable per-check summary may come before the block (plus any
voice-guide notes in live mode); the block itself is the machine-read
result.

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote
  that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready and
  a beautiful confident one can be hiding drift. Every check reads the
  diff, the evidence, and the description against the plan and the
  issue, never against formatting or length.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md`'s stated
  order and steps; report a gap instead of silently improvising around
  it.
- Unclear defaults to fail: treat `unclear` as the rubric's verdict rule
  directs; where the rule is silent, an unverifiable claim is a failing
  one. A PR you cannot verify from the package is a PR that is not
  ready to submit.
