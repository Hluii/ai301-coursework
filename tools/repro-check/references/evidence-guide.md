# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->

### Where it lives: the exact spot in a practice submission, and on GitHub, 

The Environment: line at the top of the repo report. Compare it with the issue's own Environment: line.

In the issue body, it's usually denoted by `environment:, version:, or OS:`. If one exists, then read the whole line to parse out the environment or version number of the application. 

### where that proof shows up

It names the version and OS the repo's bug template asks for, and matches the issue's version or says why it doesn't.


## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->

### Where it lives: the exact spot in a practice submission, and on GitHub

The numbered (or clearly ordered) steps list in the repro report, read against the issue's own "steps to reproduce" or minimal-reproduction code. On GitHub, this is the issue body's repro block or a linked minimal-reproduction (playground link, gist, repo). In a package bundle, it's the "Issue" section's steps compared with the "Candidate repro report" section's steps.

### What good looks like

Every action a stranger needs is written down in order: each command run, each config file's contents, each click, starting from a named starting state (a fresh clone, a specific config, a specific flag) through to the observed result. No jump from setup straight to "see error" with a step missing in between. The steps also have to actually match the issue's trigger, not a nearby variant: if the issue's repro uses a specific syntax, flag, or input shape (e.g. an offset-from-end range, a `=` separator, a specific selector combination), the report's steps use that same shape, not a substituted one that looks similar. A step that silently swaps an argument, a config key, or an operator for a different one is not a followable step; it's a different bug. If the report reproduces on a private resource (an internal monorepo, an unshared config, credentials only the author has), a stranger cannot re-run it even if the steps are numbered. Treat that as unfollowable.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->

### Where it lives: the exact spot in a practice submission, and on GitHub

The pasted output/error/log excerpt in the repro report (a terminal transcript, a log tail, a rendered CSS/HTML diff, a screenshot description), read against the issue body's own "current result" / error text / stack trace, plus any maintainer comment in the thread that narrows or restates the exact symptom. In a bundle, this is the repro report's shown output compared with the Issue section's "current result" and any thread highlight that pins down the trigger.

### What good looks like

The excerpt shows the *same* symptom the issue names: same error type and message, same exit code, same missing/incorrect field, same log line repeating, not an adjacent failure that a skim could mistake for it. Watch for the trap shapes this course's set is built around: a graceful validation error standing in for a reported crash/panic (different exit code, no crash), a config-only or syntax error standing in for the reported logic bug, or an artifact that only proves the tool *ran* (a version banner, a normal session list) without showing the specific broken behavior at all. A control run (the same command with the fix condition, or on the non-buggy path) that contrasts with the failing run is a strong positive signal: it proves the author isolated the trigger rather than just noticing *some* difference. No output excerpt at all, when the check needs one, is `unclear`, not a pass.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->

### Where it lives: the exact spot in a practice submission, and on GitHub

The report's stated "Expected" / "Actual" / conclusion lines, read against the output excerpt shown immediately above them in the same report. Also the claim comment's confidence language ("I verified...", "guaranteed", "this is definitely caused by...") read against whatever evidence the report actually includes for that claim.

### What good looks like

What's claimed is exactly what the shown artifact supports, no more. "Reproduced" needs a shown artifact that matches the issue's symptom (see Behavior shown); "could not reproduce" is a full pass when the author ran the real attempt, shows the clean/non-failing output they got, and names what they think differs (a version, a platform, a config shape). An honest negative is not a weaker result than a positive one. Fails look like: a stated crash/failure when the pasted output shows a normal exit or success; a root cause asserted ("this is a race condition", "I verified this race condition") with no traced code or reproduced evidence backing it; or certainty language ("guaranteed", "100% reproducible") sitting on top of zero artifacts. If there's no artifact at all to check the claim against, the honesty check has nothing to verify. Grade accordingly rather than assuming good faith.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->

### Where it lives: the exact spot in a practice submission, and on GitHub

The candidate claim comment and repro report text, read against three things in the repo-facts block (or, live, the repo's actual CONTRIBUTING.md / issue template / AI-use policy): (1) the bug report template's asked-for fields, (2) the contribution policy's stated rules, and (3) any stated AI-use disclosure requirement specifically. A repo's AI policy ranges from absent, to permissive, to strict-with-required-disclosure. Read the exact wording rather than assuming a default.

### What good looks like

The claim comment states specific intent (what the author will do next, e.g. "test the draft patch against both configurations") in the author's own words, matching the voice guide's rules where one applies. It fails as boilerplate when it's generic enthusiasm interchangeable with any other issue ("Great project! Assign me, I'll fix it in 2 days guaranteed!", a plain "+1"/"same here" with no reproduction attached), or when it over-promises a guaranteed fix or timeline the author cannot know yet. Separately and non-negotiably: when the repo's stated policy requires disclosing AI assistance, the comment(s) must disclose it (naming the tool and the extent of assistance). Silence on this is a fail regardless of how good the repro otherwise is, since disclosure is a wall, not a preference. When the policy is silent or merely permissive, no disclosure is required and its absence is not a mark against the package.
