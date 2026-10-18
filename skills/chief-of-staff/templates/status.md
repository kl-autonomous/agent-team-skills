# Status

Kept by `{{CHIEF}}`, the chief of staff (see docs/chief-of-staff.md).
Last updated: {{DATE}}.

**In one line:** {{ONE_LINE}}

## Sessions

| Session | State | Doing now | Last message (one line) | Restart? |
|---|---|---|---|---|
| `{{CHIEF}}` | working | Setting up | — | No |
{{SESSION_ROWS}}

State is one of: working, waiting for (whom), idle, stopped.
"Restart?" is yes or no, with the reason: the session said so, its
context is long, it's stuck or looping, or it stopped.

## Waiting for the owner

Most important first. Each one says what it blocks. Numbers stay the
same until the question is answered.

Nothing needs you.

## Done since last time

One line each, with where the proof is (a commit, a file, a test result).

## Next

What each session will do once nothing blocks it.
