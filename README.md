# agent-team-skills

Four Claude Code skills for running a team of Claude sessions without
reading every chat.

**[See how they change your workflow →](https://kl-autonomous.github.io/agent-team-skills/)**

> **What they need:** Claude Code. The skills where sessions talk to each
> other (build-review-loop, chief-of-staff) need the Claude desktop app's
> Code tab. owner-desk also needs Claude artifacts with a database
> (claude.ai or the desktop app).

| Skill | What it does | Useful from |
|---|---|---|
| [restart-ready](skills/restart-ready/) | Keeps a session's state in a few short .md files, so it can be thrown away and restarted fresh whenever its context grows, and lose nothing. It says when it's a good moment to restart, and writes the prompt for its own replacement. | one session |
| [build-review-loop](skills/build-review-loop/) | A builder builds one step at a time; an independent reviewer checks the exact commit in its own copy, re-runs everything, and answers PASS, FIX or STOP. An optional tester tries to break the risky parts. | two sessions |
| [chief-of-staff](skills/chief-of-staff/) | One session between you and the rest. It passes your words on exactly as you wrote them, gathers every session's questions into one numbered round, keeps hand-offs moving, keeps a status page, and tells you when a session should restart. | three or more sessions |
| [owner-desk](skills/owner-desk/) | A live board for everything only you can decide or do. You answer from your phone whenever you like, your answer wakes the agent, and it acts and archives the item. | any time decisions pile up |

They stack: every session is restart-ready; a builder and a reviewer form
the loop; a chief of staff runs the team for you; and the desk holds what
waits for you.

![The owner desk's overview: two items need you, one is answered, one is parked for later](skills/owner-desk/screenshots/overview-light.png)

## Install

As a plugin, in Claude Code:

```text
/plugin marketplace add kl-autonomous/agent-team-skills
/plugin install agent-team-skills@agent-team-skills
```

Or copy the skills you want into your personal skills folder:

```bash
git clone https://github.com/kl-autonomous/agent-team-skills.git
```

```bash
cp -R agent-team-skills/skills/restart-ready ~/.claude/skills/
```

## Where they come from

All four came out of a real project: an iPhone app built by a team of five
Claude sessions (a builder, a reviewer, a tester, a feedback reader and a
manager). It went from its first decision to a beta with outside testers in
nine days, with the owner making every decision and writing no app code.

- The reviewer sent work back 7 times, with 12 blocking problems, before
  anything merged.
- Sessions were restarted 12 times, with nothing lost.
- The board the owner asked for closed 18 of its first 30 items on its
  first day.

How each one has been tested:
- **owner-desk:** tested end to end on a demo board: posting, waking the
  agent on an answer, archiving, and answering questions, at phone width
  and in dark mode.
- **The other three:** written up from daily use on that project, and
  tried together on a small practice project.

## License

MIT. See [LICENSE](LICENSE).
