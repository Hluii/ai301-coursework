# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a new grad with academic coding experience, doing my first real-world
open source work. Strongest in Python/C++; also comfortable in SQL, React,
JS, Java. Readers should expect careful, verified claims.

## Rules I write by

### Rule: State cause only as far as I traced it
A plan comment has to name a cause — that's the point of the plan — but I say how I know it,
not just assert it. If I traced the cause in the code or ruled out alternatives with a repro
step, I say which. If I'm inferring from the symptom alone, I say that instead of stating it
as settled fact.

- Wrong: "This is a race condition in the connection pool."
- Right: "The refresh callback in `sync_controller.go` doesn't add the commits context to its
  post-push scope — confirmed by stepping through it; that's why the color only updates on
  re-entry."

### Rule: Say what I didn't check
If I only tested on one OS/version, I say so instead of implying broader coverage.

- Wrong: "This bug happens on save."
- Right: "This bug happens on save, tested on macOS 14 / Python 3.11 only."

### Rule: Name the scope boundary out loud
Repro comments only reported what happened; a plan comment commits to a change, so I say what
I will touch and what I won't, so a maintainer can object to the boundary before I build on it.

- Wrong: "I'll clean this up and send a PR."
- Right: "I'll fix the refresh callback in `sync_controller.go`; I won't touch how push status
  is computed or other views' refresh behavior."

## Things I never post
- "This should be an easy fix" (I don't know that yet)
- Claiming I reproduced something when I only read the code and it looked plausible
- A plan comment that commits to a fix without saying what's in scope and what isn't
- "Same approach as above" standing in for my own plan on a shared issue