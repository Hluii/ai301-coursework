# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

Where it lives: eval bundle — the "Cause:" line (or equivalent opening
statement) of the `## Candidate plan` section, read against every
numbered step and any control/isolation step in the `## Repro
evidence` block. Live — the plan.md diagnosis section, read against
the student's own posted repro comment (or the house repro pack).

What good looks like: the stated cause explains every observation the
repro evidence reports, including a control step (a variant run with
one thing held constant, or a step showing the "blamed" code path
wasn't even exercised). A diagnosis that a control step rules out
fails this check no matter how confidently it's argued — polish is not
evidence. If the repro evidence shows only the failing behavior with
no control step, a plausible cause consistent with that behavior is
enough.

## Scope

Where it lives: eval bundle — the "Change:" line and any "In:"/"Out:"
language in `## Candidate plan`, read against the issue's body and
title. Live — plan.md's scope section (what you'll change / what you
won't) and the files list.

What good looks like: one bounded change at the fix site(s) the
issue's behavior requires. Adjacent temptations (a refactor "while
you're in there," a migration, a new option, a generalization) are
either absent or explicitly named and excluded. A plan that performs
the right fix inside a larger rewrite still fails: the rewrite is the
scope, not the fix.

## Executability

Where it lives: eval bundle — the "Change:" line's named file(s)/area
and its stated approach in `## Candidate plan`. Live — plan.md's
"files you'll touch" and "approach" sections.

What good looks like: a specific file or location, and one chosen
approach (not a menu of options left for later). A stranger reading
only the plan could open the right file and start. Phrases that mark a
real decision as still pending ("whichever is easier," "not sure which
layer," "somewhere in the parser") fail this check even if a file is
named elsewhere in the plan.

## Test plan

Where it lives: eval bundle — the "Test:" line in `## Candidate plan`,
read against the numbered steps and artifacts in `## Repro evidence`.
Live — plan.md's test plan, read against the student's own unit-2
repro steps.

What good looks like: the test plan names a specific repro step (or a
check built the same way) and the specific change in observable output
that will mark success ("at step 3 the color must flip without leaving
the view"). A vague standard with no named observable ("should feel
fast," "run the full test suite," "nothing else should feel broken")
fails even when the rest of the plan is strong.

## Honesty

Where it lives: eval bundle — a risks/unknowns line in `## Candidate
plan`, if present. Live — plan.md's risks and unknowns section, and,
after a build, the `## Deviations` heading.

What good looks like: a stated unknown names something the author
genuinely couldn't verify (an untested platform, a deferred edge case,
an assumption the repro evidence doesn't fully pin down) rather than
restating the plan's confidence. A deviation recorded in plan.md after
the build, in the student's own words (including "nothing changed"),
counts as honest; a deviation that shows up only in the diff does not.

## Comms

Where it lives: eval bundle — the full text of `## Candidate plan
comment`, read against `## Thread highlights` (explicit maintainer
direction, if any) and the repo-facts block's stated contribution
policy and AI-use disclosure requirement. Live — the draft plan
comment, read against the live issue thread and the repo's
CONTRIBUTING/AI-policy docs per `scope.md`'s house rules.

What good looks like: the comment reflects any explicit direction a
maintainer already gave in the thread (it does not propose an approach
the thread steered away from, and it does not silently re-litigate a
settled discussion). When the repo-facts block states the repo
requires disclosing AI assistance, the comment discloses it in the
author's own words. Boilerplate ("same approach as above," generic
reassurance with no specifics) reads as not thread-aware even if the
plan itself is sound.

An empty thread (no comments) or a repo-facts block stating no AI
policy is not missing evidence to leave `unclear` — it is conclusive
evidence that nothing is required on that half of the check, so that
half passes. Reserve a `fail` here for an actual contradiction (thread
direction the comment ignores) or an actual omission (a stated
disclosure requirement the comment doesn't meet).
