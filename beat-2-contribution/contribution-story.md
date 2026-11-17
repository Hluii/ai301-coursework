# Contribution: Add tests for media.validate_url (allow-list and private-address check)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Hilary Lui
**GitHub Username:** Hluii
**Issue:** https://github.com/blixvip/BlendClone/issues/5
**Pull Request:** https://github.com/blixvip/BlendClone/pull/40

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built the issue-scout and tuned it over four scans**

I built the issue-scout: four `gh search issues` queries traced to my fit profile (Python docs, tests and bugs, TypeScript docs), ranking filters that drop assigned issues and order by fewest comments, and a graded top five. The first two scans accepted almost everything, because the results were brand-new issues opened by repo owners seeding their own backlogs, so I added date bounds to the queries and a required `outside_activity` rubric check (a non-owner PR or comment within 90 days). Those fixes did not show up in scans 2 and 3, and the session transcript showed why: the reruns resumed the same session and reused the old queries and rubric from memory. Running scan 4 in a fresh session (`--no-session-persistence`) applied them, and the scout then rejected one issue that an open PR already covered. I graded two issues I found myself in blixvip/BlendClone, and both were accepted.

**Unit 6: Held my claim comments to my voice guide by hand (no hook or memory file)**

I did not install the voice-guard hook or write a `CLAUDE.local.md` memory file; I did this work compressed into a few days. Instead I used the rules in my voice guide ("don't guess at cause", "say what I didn't check", never post "this should be an easy fix") to shape my claim comments with Claude Code's help, and I only claimed after running the setup and the baseline tests myself, so the comment states what I observed (160 passed, 9 skipped, no tests for `validate_url`). What fell short: `repro-check` is scoped to the sandbox repo, so it could not grade a BlendClone claim, and my first claim (on #2) was overtaken by a stranger's PR within hours. What I changed: I checked the repo's open PRs and the code before claiming, which is why #5 held.

**Unit 7: Ran the project's own checks by hand before opening my PR (no ship-gate)**

I did not install the ship-gate or write a `ship-manifest.md`. Before opening PR #40 I ran the checks the repo uses myself: the new tests alone (`23 passed`), the full suite (`183 passed, 9 skipped`, the skips traced to FFmpeg not being installed), and `grep -rn validate_url tests` before and after. I used Claude Code to draft the PR description from the repo's PR template and then answered every item in it. Nothing blocked me, but the checks were manual, so nothing would have stopped a PR with a missing piece; the repo's CI (pytest, passed) was the only automatic check. The one thing that came up was the Discord invite failing, which I noted in the PR.

**Unit 8: Read the PR thread and replied by hand (no PR monitor)**

I did not install the PR monitor or write a playbook. My PR was merged about seven hours after I opened it (opened 2026-11-16 18:03 UTC, merged 2026-11-17 01:20 UTC), with an automatic acknowledgement in between and no changes requested, so there was no stale-PR or review-reply situation for a monitor to draft. I read the maintainer's merge comment myself and replied by hand, thanking them and declining their suggested follow-ups (#22 and #26) because I am traveling. Nothing broke, and I know because the PR merged with CI green and no open review comments.

---

## Why I Chose This Issue

**Claim comment:** https://github.com/blixvip/BlendClone/issues/5#issuecomment-6923724801 (posted 2026-11-16, by Hluii)

*Earlier claim, dropped:* I first claimed BlendClone #2 (https://github.com/blixvip/BlendClone/issues/2#issuecomment-6913275453). A stranger opened PR #37 for it about 3.5 hours later, so I stepped back and took #5.

I chose this issue because writing test cases is a low-risk way into a new codebase: I can copy the structure of the existing test files, and I do not have to worry about breaking a feature, since I am only adding tests. The "done when" was also clear: `grep -rn validate_url tests` should stop coming back empty, and the new tests should pass offline. The repo itself interested me too. I have a little Blender experience, and turning a reference video into an editable Blender file is a cool idea.

I found the repo by looking at the "contributors wanted" hub issue (#14) and reading the good first issues one by one, then ran my scout on the two that seemed easiest and most relevant (#2 and #5). Both came back accept, with all nine rubric checks passing, including `outside_activity` (merged PRs from three non-owner accounts) and a repo-wide search for hidden PRs. I found this repo myself on GitHub; it did not come from the Issue Selection site. Before the scout, I had run four scans of my own queries (see the Toolkit entries below), but those mostly returned brand-new issues that repo owners had filed for themselves, so none of them became my issue. #2 was my first pick and I lost it to PR #37. Two other candidates were out too: WebDevTools #402 failed `unclaimed` (a stale open PR from April that the issue page did not show), and BlendClone #23 was accepted by the scout but already fixed on `main`, which I only caught by reading the code. I re-checked #5 for competing PRs and existing tests before I claimed it.

Reflection on the scout: it held up on the checks it has (it rejected two candidates correctly) but it missed things a human had to catch. It could not see a stale PR that was not linked to the issue, it had no check for "already fixed on the default branch", and its `outside_activity` check passes on contractor or owner-adjacent accounts. The scan results also leaned toward tiny docs-only issues because I ordered by fewest comments. I patched `search-strategy.md` and `rubric.md` for the first problems and I would add the other two checks next.

**Toolkit**

- Tool: issue-scout (my search strategy `search-strategy.md` and rubric `rubric.md`), used for four scans of my own queries and for direct grades of BlendClone #2, #5 and #23, WebDevTools #402 and HHS #5289.
- What it produced: the scans (all five accepted at first, three of five after my fixes), and a nine-check accept for #5 with a clean hidden-PR search.
- Difficulties: the scout ran inside a resumed session and silently reused old queries and an old rubric until I started a fresh session; it missed a stale unlinked PR on #402 and an already-fixed bug on #23.
- What changed: I added date bounds and an `outside_activity` check, ran later scans with `--no-session-persistence`, and noted two more rubric checks to add (repo-wide PR search in `unclaimed`, and "not already fixed on the default branch").

---

## Understanding the Issue

**Problem Description**

`validate_url()` in `app/media.py` is the only check between a link a user pastes and `yt-dlp` making outbound network requests. It limits links to HTTPS and to YouTube, Vimeo and X hosts, and blocks hostnames that resolve to private or internal addresses. No test in the repo exercised it, so a change that broke any of those rules would not be noticed.

**Expected Behavior**

There should be automated tests that pin down each rule: allowed hosts pass, anything else is rejected with the right error message, and private addresses and DNS failures are rejected.

**Current Behavior**

`grep -rn validate_url tests` printed nothing before my change. The server tests only post a YouTube link with background jobs turned off, so the allow-list and private-address check were never run.

**Affected Components**

`app/media.py` (the function under test, not changed) and a new `tests/test_media.py`. The function is called from `download()` in the same file and from `app/server.py`.

**Toolkit**

- Tool: Claude Code, used to read the issue and `validate_url()` and explain what each of its three checks does.
- What it produced: a plain-language breakdown of the function (scheme, credentials and port; host allow-list; private-address check) that I used to plan the test groups.
- Difficulties: none met.
- What changed: nothing.

---

## Reproduction Process

**Environment Setup**

Windows with WSL2 (Linux), Python 3.12.3, pytest 9.1.1, a virtual environment made with `python3.12 -m venv .venv`, and dependencies installed from `requirements.lock.txt`. I cloned my fork and ran `python -m pytest`. Blender, FFmpeg and a ChatGPT account are not needed; the test suite mocks them. Challenges: the project Discord that `CONTRIBUTING.md` recommends would not let me join ("Unable to accept"), so I said so in the PR and the maintainer replied that the invite works and it was probably a temporary Discord problem. The nine FFmpeg-dependent tests are skipped because FFmpeg is not installed here; I confirmed that was the cause.

**Steps to Reproduce**

1. `git clone https://github.com/<my-username>/BlendClone.git && cd BlendClone`
2. `python3.12 -m venv .venv && .venv/bin/python -m pip install -r requirements.lock.txt`
3. `.venv/bin/python -m pytest` gives `160 passed, 9 skipped, 2 warnings`.
4. `grep -rn validate_url tests` prints nothing: the function has no tests.

**Reproduction Evidence**

- **Commit showing reproduction:** none. This issue is missing tests, so there is no failing behavior to commit; the evidence is the command output above, quoted in my claim comment (https://github.com/blixvip/BlendClone/issues/5#issuecomment-6923724801).
- **Screenshots/logs:** the pytest summary and the empty grep, in the claim comment; the "after" grep is in the PR description (`validate_url` now appears in `tests/test_media.py`).
- **My findings:** the gap is real and nobody else was working on it: I checked the open PRs (#37 is for a different issue) and the repo for existing tests before claiming. The issue text said nine tests fail without FFmpeg; they are skipped now because an earlier merged PR (#20) made them skip.

**Toolkit**

- Tool: Claude Code, which gave me the setup and `grep` commands and told me what output to expect; I ran them myself.
- What it produced: a baseline I could quote (160 passed, 9 skipped) and a check that no PRs or tests already covered `validate_url`.
- Difficulties: the Discord invite failed for me; nothing else met.
- What changed: I noted the Discord problem in the PR instead of blocking on it.

---

## Solution Approach

**Analysis**

`validate_url()` in `app/media.py` (lines 18-30) rejects a URL that is not HTTPS, has credentials, or uses a port other than 443; rejects hosts outside YouTube, Vimeo and X (subdomains allowed); then resolves the host with `socket.getaddrinfo` and rejects non-public addresses. The function itself is correct as written; what is missing is tests.

**Proposed Solution**

Add one new test file, `tests/test_media.py`, with no change to `app/media.py`. Replace the DNS lookup (`app.media.socket.getaddrinfo`) with a fake via pytest's `monkeypatch`, so the tests run offline and give the same result every time. I deliberately did not change `validate_url()` itself.

**Implementation Plan**

1. Create `tests/test_media.py` and import `validate_url` from `app.media`.
2. Write cases for scheme, credentials and port rejection.
3. Write cases for the host allow-list: YouTube, Vimeo and X with their subdomains, and look-alike hosts that must be rejected.
4. Monkeypatch `getaddrinfo` to return private and public addresses and assert the private-network rejection and the pass-through; add the DNS-failure case.
5. Run `python -m pytest` and show the new tests passing.

As built: 23 tests, covering the cases listed in the issue plus extras (youtu.be, vimeo.com, x.com, twitter.com, `youtube.com.evil.example`, a private address mixed in with public ones, and an empty lookup result). The plan changed in one way: it was first written for issue #2 and rewritten for #5 after I switched.

**Toolkit**

- Tool: Claude Code, used to draft the plan from my reading of the issue and `validate_url()`.
- What it produced: the file, function and test-group plan above, first for #2 and then for #5.
- Difficulties: the first plan was for an issue I lost, so the first version was thrown away.
- What changed: I rewrote the plan for #5 and recorded the switch in "Why I Chose This Issue".

---

## Testing Strategy

Two observable checks, run before and after my change:

1. **Coverage check.** Before: `grep -rn validate_url tests` prints nothing. After: it lists `tests/test_media.py` (the import and each call), shown in the PR description.
2. **Test runs.** `.venv/bin/python -m pytest tests/test_media.py -v` gives `23 passed in 0.21s` offline. The full suite goes from `160 passed, 9 skipped` before to `183 passed, 9 skipped` after (160 + 23 = 183). CI on the PR ran `pytest` and passed, and the maintainer reported 192 tests in total once merged.

**Toolkit**

- Tool: pytest and `grep`: I ran the new tests, then the full suite, and used `grep -rn validate_url tests` to confirm the function was now covered.
- What it produced: 23 tests in `tests/test_media.py`, all passing, and a full-suite result of 183 passed, 9 skipped.
- Difficulties: none met; the 9 skipped tests needed an explanation, which I traced to FFmpeg not being installed.
- What changed: nothing.

---

## Implementation Notes

**2026-11-14 to 2026-11-15**

Built and tuned the scout over several scans. The first scans accepted everything and returned mostly brand-new, owner-filed issues. I found that reruns inside the same session reused old files, and fixed it by starting fresh sessions.

**2026-11-16**

Found BlendClone through its contributor hub (#14) and read the good first issues. Ran the scout on #2 and #5 (both accept) and claimed #2. About 3.5 hours later a stranger opened PR #37 for #2, so I switched. The scout rejected WebDevTools #402 (a stale unlinked PR) and accepted #23, which I found was already fixed on `main`. I then re-checked #5 for competing PRs, ran the baseline (160 passed, 9 skipped), and claimed #5. I created the branch `issue-5-validate-url-tests`, wrote `tests/test_media.py` (23 tests, `getaddrinfo` monkeypatched) and opened PR #40 at 18:03 UTC.

**2026-11-17**

I tried the Discord invite again and it still failed, so I noted that in the PR. The maintainer merged PR #40 and suggested follow-up issues #22 and #26; I replied that I am traveling and left them open.

**Toolkit**

- Tool: git (branch, commit, push), pytest, and Claude Code, which helped me draft my claim comment and plan.
- What it produced: the branch `issue-5-validate-url-tests`, one commit (`test: add tests for media.validate_url ...`), and the claim comment on #5.
- Difficulties: my first claim, on #2, was overtaken by a stranger's PR within hours.
- What changed: I switched to #5 and rewrote the plan for it.

---

## Pull Request

**PR Link:** https://github.com/blixvip/BlendClone/pull/40

**PR Description:** Adds `tests/test_media.py`, the first tests for `app.media.validate_url()`, with `Closes #5`. It has a table of the 23 cases (allowed hosts; scheme, credentials and port rejection; look-alike hosts; private addresses; a private address mixed with public ones; an empty lookup; DNS failure), the offline test output, the full-suite result (183 passed, 9 skipped, with the skips traced to FFmpeg), the repository checklist, and a note that I could not join the Discord the contributing guide recommends.

**Maintainer Feedback:**

- 2026-11-16: an automatic reply said a maintainer would look at the PR before anything was merged. A Copilot review attempt did not run (insufficient quota on the requester's side).
- 2026-11-17: the maintainer merged it, noted CI was green and the suite had 192 tests, said the Discord invite works on their side (probably a temporary Discord problem), and suggested issues #22 or #26 as next steps. I replied that I am traveling and would leave both open.

**Status:** Merged (2026-11-17).

**Toolkit**

- Tool: Claude Code, which helped me draft the PR description from the repository's PR template.
- What it produced: a description with a table of the 23 cases, the test output, the checklist, and `Closes #5`.
- Difficulties: I could not join the project Discord that `CONTRIBUTING.md` recommends before opening a PR.
- What changed: I said so in the PR description instead of waiting, and the maintainer replied that it was fine.

---

## Learnings and Reflections

**Technical Skills Gained**

- The branch, commit, push, pull request and merge workflow, and why you never commit straight to `main`.
- What monkeypatching is and how it makes code that depends on the network testable offline: I replaced `app.media.socket.getaddrinfo` with a fake so the private-address check could be tested with no DNS.
- Following a project's conventions: conventional-commit message style (`test: add ...`), the PR template, and linking the issue with `Closes #5`.

**Challenges Overcome**

- Losing my first issue: I claimed #2 and a stranger opened a PR within hours, so I re-checked candidates, took #5 and opened my PR about 40 minutes after claiming it.
- The Discord invite would not work. I said so openly in the PR instead of blocking on it, and the maintainer was fine with that.
- Communicating with the maintainer: I claimed with evidence of what I had actually run, and I declined the follow-up issues politely instead of leaving them hanging.

**What I'd Do Differently Next Time**

Before claiming, I would also search the repo's open and recent PRs for the issue number and read the code the issue cites, which would have caught the stale PR on #402 and the already-fixed bug on #23 sooner. I would also claim only once I have run the setup and reproduction, as I did for #5. I would also make my PR turn around faster. Someone opened a PR for #2 within hours of my claim there, so next time I would get my change written and my PR open as soon as possible after claiming instead of leaving a gap.

---

## Resources Used

- `CONTRIBUTING.md` and the PR template in blixvip/BlendClone, which told me how to set up, run the tests and open the PR.
- Issue #5 itself, which listed the cases to test and the "done when".
- The existing files under `tests/` in the repo, which I used as a guide for structure.
- The maintainer's replies on PR #40, which confirmed the Discord invite works and pointed to the follow-up issues.
