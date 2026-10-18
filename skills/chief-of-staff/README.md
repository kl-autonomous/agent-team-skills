# chief-of-staff

A Claude Code skill that puts one session between you and the rest of your
Claude sessions, so you talk to one chat instead of five.

> **What it needs:** the Claude desktop app (Code tab), where sessions can
> see and message each other. It doesn't work in plain terminal Claude Code.

## The problem

Once you run more than two or three Claude sessions on one project, you
become the switchboard:

- you copy messages from one chat to another, and shorten them as you go;
- questions are spread over every chat, each asked in its own way;
- you can't tell who is working, who is waiting, and who is waiting for you;
- a session stops at a usage limit and nobody notices for hours;
- long chats get slow and forgetful, and restarting one loses what only
  lived in that chat.

## What the chief of staff does

It's one more session, and its only job is the team's communication. It
never writes code, reviews or designs.

| It… | So that… |
|---|---|
| passes your words on exactly as you wrote them, starting "the owner says:" | instructions aren't changed by a summary on the way |
| looks up facts itself, and asks you only for decisions, in one numbered round, each with a recommended answer | you can reply "1 yes, 2 no" from your phone |
| watches the hand-offs, checks every session after each report, and resends work that stalled | nothing sits idle without anyone noticing |
| keeps `docs/status.md`: who is doing what, what waits for you, what's done, what's next | you can see the whole team in one page |
| tells you when a session should restart, and has the session write the prompt for its own replacement | a restart loses nothing |
| sends you a notification only when something finishes, needs you, or risks data, devices, accounts or money | you're not pinged all day |

The rules it never bends:
- Nothing is installed, bought, published or deleted without your word in
  chat.
- A session's report isn't proof until it has seen the file or result.

## An example team

A made-up project: a small recipe app with three sessions.

| Session | Job |
|---|---|
| `builder` | builds each feature, with tests |
| `reviewer` | checks each feature in its own copy of the code |
| `feedback` | reads beta testers' notes and sends bugs to the builder |
| `chief-of-staff` | everything between you and the three of them |

A message from you:

> tell the builder to ship the search fix once the reviewer passes it

reaches the builder as:

> the owner says: "tell the builder to ship the search fix once the
> reviewer passes it"

When the builder and the reviewer both have questions, you get one round:

> 🔴 3. Show recipes without photos in search? The builder recommends
> yes: about a third have no photo yet.
>
> 🔴 4. Restart the reviewer now? Its chat is long and everything is
> committed. Yes?

## Install

Copy the skill into your personal skills folder:

```bash
git clone https://github.com/kl-autonomous/agent-team-skills.git
```

```bash
cp -R agent-team-skills/skills/chief-of-staff ~/.claude/skills/
```

## Use it

1. In your project's folder, start a session and say: "set up a chief of
   staff for my sessions".
2. It looks at your sessions and files, then asks you a few numbered
   questions: the team, its own name, who restarts sessions, and what it
   should notify you about.
3. It writes `docs/chief-of-staff.md` (the team, your choices and its
   prompt) and `docs/status.md`.
4. You create the chief's session with the name and prompt it gives you,
   and add one line to every other session's prompt.

From then on you talk only to the chief of staff. Say "status" at any time
to see the whole team.

## Works well with

- [owner-desk](../owner-desk/): a live board for everything only you can
  decide or do. With both installed, the chief of staff posts everything
  it's waiting on you for to the desk, and acts on your answers there.
- [build-review-loop](../build-review-loop/): the builder and reviewer
  the chief of staff keeps moving.
- [restart-ready](../restart-ready/): what lets every session, the chief
  included, restart without losing anything.

## License

MIT
