# side-session

A Claude Code skill that fences off a helper session so it can work on
one topic next to your team of sessions without adding work to that
team.

> **What it needs:** Claude Code, in any form. It matters most in the
> Claude desktop app, where sessions can message each other and a side
> session could otherwise reach the team.

## The problem

You run a team: a builder, a reviewer, maybe a tester, and a chief of
staff you talk to. Then you want help with one more thing: a store
filing, a legal check, a design idea, your own notes. You start a session
for it. Left alone, that session drifts into the team's work:

- it sees a bug while reading the code and fixes it, on the builder's
  branch;
- it messages the builder "by the way…", and the builder now has two
  owners;
- the chief of staff adds it to the status page, broadcasts rules to it,
  and tracks its restarts, paying a turn for each;
- it lives for weeks, and every old turn rides along into every new one.

Reading the same project costs nothing. Writing where the team writes,
and speaking to the team's sessions, is what breaks things.

## What the skill makes a side session do

| It… | So that… |
|---|---|
| talks to you, not to the team | the team hears one voice, yours |
| reads the project freely, never the team's chats | it knows what the files say, without paying for a chat it doesn't need |
| keeps its own notes in its own folder, outside the repo | the team's commits never collide with it, and it can be restarted from the notes |
| never writes to the team's files, branches or desk, never messages the team's sessions | the reviewer's diff stays true and the chief's "relay verbatim" rule keeps meaning something |
| hands results to you as a proposal file with one line to paste | you decide what reaches the team, at the cost of pasting one line |
| says when a fresh session would be cheaper than reopening it | a helper used in bursts doesn't drag weeks of context around |

## An example

A made-up project: a recipe app built by `builder`, `reviewer` and
`chief-of-staff`. The owner wants help with the app's listing in another
country's store, which needs a filing the team knows nothing about.

They start a session and say: "set up a side session for the Japan
store filing". It asks five numbered questions (the topic, its notes
folder, what it may read, how results reach the team, which model),
writes `~/Projects/japan-store-notes/` with its rules and an empty
hand-over file, and gets to work.

Two days later it finds the privacy page needs one more sentence. It
doesn't edit the page and doesn't message the builder. It writes
`~/Projects/japan-store-notes/proposals/2026-10-20-privacy-sentence.md`
and says:

> The filing needs one sentence on the privacy page. The proposal is at
> ~/Projects/japan-store-notes/proposals/2026-10-20-privacy-sentence.md.
> To pass it on, paste this to the chief of staff: "The proposal at
> ~/Projects/japan-store-notes/proposals/2026-10-20-privacy-sentence.md
> is my words; send it to the builder."

The owner pastes it, or doesn't. The team never hears from the side
session itself.

## Install

```bash
git clone https://github.com/kl-autonomous/agent-team-skills.git
```

```bash
cp -R agent-team-skills/skills/side-session ~/.claude/skills/
```

## Use it

1. Start a session in your project's folder and say: "set up a side
   session for X".
2. Answer its five numbered questions ("yes" accepts each default).
3. Work with it as you would with any session. When it gives you a
   proposal and a line to paste, you decide.

## Works well with

- [restart-ready](../restart-ready/): the notes folder is a side
  session's state file; it writes its own restart prompt.
- [chief-of-staff](../chief-of-staff/): the chief leaves side sessions
  alone; you are the only bridge.
- [build-review-loop](../build-review-loop/): a side session never
  commits to the loop's branches.
- [owner-desk](../owner-desk/): a side session never writes to the
  desk.

## License

MIT
