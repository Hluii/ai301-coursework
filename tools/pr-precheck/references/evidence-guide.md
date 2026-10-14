# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives: in an eval bundle, the "Plan context" section states the
accepted plan's files, boundary, and test plan; the "Candidate PR"
section's diff shows what actually changed. In live mode, your own
`plan.md` (including its `## Deviations` section) is the plan, and
`git diff main...HEAD` (three dots) is the diff. What good looks like:
every changed file and hunk falls inside the plan's stated files and
boundary, or is covered by an explicit deviation note naming what
changed and why. Silent drift runs in two directions: a diff that does
more than the plan names (an unrequested rewrite, a bundled unrelated
feature) with no deviation note, or a diff that quietly does less than
the plan promised (a scoped-in file never touched) with no note
explaining the gap. The description's own fidelity claims ("implements
the plan exactly", "no functional changes outside X") belong here too:
when the diff contradicts a claim like that, it is drift, not a wording
problem.

## Test evidence (harness category: not-tested)

Where it lives: in an eval bundle, the "Test evidence" subsection of the
candidate PR, read against the plan context's test plan and the issue's
own reported behavior. In live mode, your `test_evidence.md` (the
before/after repro re-run plus the repo's required checks), read against
your `plan.md`'s test plan and your issue's original repro steps. What
decisive looks like: an observable condition named (an error message, an
exit code, a printed value) shown before the change and shown resolved
after, using the same condition the issue itself reports, not an
adjacent symptom. "Tests pass" or "works now" with nothing named is not
decisive. A required repo check that genuinely has nothing to run (for
example, an empty integration suite) is decisive when a concrete, named result is given for it (which command
ran, and its specific outcome: a pass/fail state, a test count, or the
"no tests ran" message), rather than silently omitted or stated as a
vague "all checks pass" with no command named. A template requirement
that names a code property rather than a command (a formatting
convention with nothing to run, an i18n string the diff already adds)
is satisfied by the diff itself, not by a separate pasted run.

## Diff quality (harness category: unreviewable)

Where it lives: the unified diff itself and the commit list, in the
bundle's "Candidate PR" section or in `git diff main...HEAD` and
`git log main...HEAD` live. What a reviewable change looks like: the
fix is visible on its own, and nothing unrelated rides along. Debris
tells: commented-out code left in place, debug `print`/`console.log`
lines, dead branches, pure reformatting mixed into a functional hunk,
and drive-by edits to files the plan never named. A diff with debris is
unreviewable even when the underlying fix is correct.

## Standards and comms (harness category: standards-wall)

Where it lives: in an eval bundle, the repo-facts block's PR-template
section list and its stated contribution/AI policy, read against the
candidate description. In live mode, the repo's own
`.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` (fetched
live), read against your draft `pr_draft.md`. What compliant looks
like: every template section carries real, specific content (not the
template's own placeholder text), the issue is named concretely, and
where the repo states an AI-use disclosure expectation, the description
states plainly how AI was used on the change. Boilerplate ("N/A" with no
reason, a section left as the template's own instructional text) or a
visibly ignored ask (a required checklist line never addressed) is a
standards-wall failure. Whether the description's claims match the
diff's actual content is plan fidelity, above, not this.
