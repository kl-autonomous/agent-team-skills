# restart-ready

A Claude Code skill that keeps a session ready to be thrown away and
restarted fresh at any moment, without losing anything.

> **What it needs:** Claude Code, in any form, and a project folder (a git
> repo is best). It works for a single session as well as for a team.

## The problem

A long chat gets slow, expensive and forgetful. It repeats mistakes you
already corrected, and loses track of what it promised. Starting a fresh
session fixes that, but then everything that lived only in the old chat is
gone, so people keep long chats going far too long.

## The idea

Treat the chat as short-term memory and files as long-term memory. If
everything that matters is written into a few short .md files in the
repo, a fresh session started with the same prompt reads them and carries
on. The session becomes disposable: restart whenever the context grows,
and lose nothing.

## What the skill makes a session do

- **Keep a short state file**: done, in progress, next, waiting for whom.
  History moves to a separate log, so the state file stays one screen
  long.
- **Read first, then only what the step needs.** Wide searches go to a
  sub-agent that reports back only the answer.
- **Write corrections down at once.** A rule goes in the project notes, a
  decision in the decisions file, so the next session doesn't repeat the
  mistake.
- **End each unit of work with one line**: "Good moment to restart me:
  yes", or "no need yet". Only the session can see how full its context
  is.
- **Write the prompt for its own replacement** when asked: the original
  prompt, plus the little that isn't in the files yet. You paste it into a
  fresh session.

## Install

```bash
git clone https://github.com/kl-autonomous/agent-team-skills.git
```

```bash
cp -R agent-team-skills/skills/restart-ready ~/.claude/skills/
```

## Use it

Tell a session: "work restart-ready in this project". It sets up its
state file and keeps it current. When it says it's a good moment to
restart, ask it for its restart prompt, start a new session, and paste
the prompt in.

## Works well with

- [build-review-loop](../build-review-loop/): every role in the loop is
  restart-ready.
- [chief-of-staff](../chief-of-staff/): it tracks which sessions are due
  for a restart and hands you their prompts.

## License

MIT
