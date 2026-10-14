# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment_recorded | The repro report's environment section (OS + version/commit) | Pass if OS and a version/commit number are both stated and are consistent with what the issue's stack trace or description implies (e.g. issue mentions Python 3.10-only behavior, report states a 3.10.x version). Fail if either is missing, or if stated but contradicts the issue (e.g. issue is Linux-only, report ran on Windows with no note about that). Unclear if the report gives an environment but the issue gives no signal to check it against. | Required |
| steps_complete_and_followable | The repro report's steps list, read against the claim comment's description of the bug | Pass if a stranger could execute the steps with no missing action (each command run, each file edited, each button/click named) and land on the same behavior. Fail if a step is implied but not written (e.g. jumps from "clone repo" to "see error" with no setup/run step in between). | Required |
| behavior_match_issue | The report's output/error excerpt, read against the issue's stated behavior or error text | If the report claims reproduction: pass if the excerpt shows the same symptom the issue describes (same error type/message, same observable failure), not a superficially similar one; fail if the excerpt shows a different error, a different code path, or no failure at all. If the report claims cannot-reproduce: pass if the excerpt shows the real attempt's clean/non-failing result, consistent with that claim (a clean run is expected here, not a fail). Unclear if no output excerpt is included. | Required |
| honest_outcome | The report's stated result, read against the actual output excerpt it includes | Pass if the stated outcome (reproduced / did not reproduce / partial) matches what the included evidence actually shows, including an honest "could not reproduce" backed by a clean run. Fail if the write-up claims a result the excerpt doesn't support (e.g. claims a crash but the pasted output shows success). | Required |
| comms_respects_repo | The claim comment and repro report text, read against the repo-facts block's contribution policy and any stated AI-use disclosure requirement (see `references/evidence-guide.md`'s Comms section) | Pass if the claim comment states specific, honest intent in the author's own words and, when the repo's policy requires disclosing AI assistance, that disclosure is present. Fail if the comment is generic boilerplate or over-promising with no real intent, or if a stated disclosure requirement is not met. | Required |
| words_respected | The claim comment and repro report text, read against `voice-guide.md`'s rules | Pass if the text follows the voice guide's rules (no rule broken). Fail if it breaks a rule that voice-guide.md marks as something the student never posts. | Preferred |

## Verdict rule

Accept if every `required` check grades `pass`. Any `required` check graded `fail` or `unclear` results in `reject`: unclear proof is treated as not ready to post. `preferred` checks (e.g. `words_respected`) are reported but never change the verdict.
