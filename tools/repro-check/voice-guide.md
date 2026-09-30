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

### Rule: Don't guess at cause
I state what I observed, not why I think it happens, unless I've traced it in the code.

- Wrong: "This is probably a race condition in the connection pool."
- Right: "I reproduced the crash on step 3; I haven't traced the root cause yet."

### Rule: Say what I didn't check
If I only tested on one OS/version, I say so instead of implying broader coverage.

- Wrong: "This bug happens on save."
- Right: "This bug happens on save, tested on macOS 14 / Python 3.11 only."

## Things I never post
- "This should be an easy fix" (I don't know that yet)
- Claiming I reproduced something when I only read the code and it looked plausible