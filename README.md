# agent-team-skills

Two Claude Code skills for running a team of Claude sessions without
reading every chat.

> **What they need:** the Claude desktop app (Code tab), where sessions can
> see and message each other. owner-desk also needs Claude artifacts with a
> database (claude.ai or the desktop app). Neither works in plain terminal
> Claude Code.

| Skill | What it does |
|---|---|
| [chief-of-staff](skills/chief-of-staff/) | One session between you and the rest. It passes your words on exactly as you wrote them, gathers every session's questions into one numbered round, keeps hand-offs moving, keeps a status page, and tells you when a session should restart, with a prompt the session writes for its own replacement. |
| [owner-desk](skills/owner-desk/) | A live board for everything only you can decide or do. Each item gets its own tab with a recommended answer, buttons, steps and files. You answer from your phone whenever you like, your answer wakes the agent, and it acts and archives the item. |

Use either one, or both. With both, the chief of staff puts everything it's
waiting on you for onto the desk.

![The owner desk's overview: two items need you, one is answered, one is parked for later](skills/owner-desk/screenshots/overview-light.png)

## Install

As a plugin, in Claude Code:

```text
/plugin marketplace add kl-autonomous/agent-team-skills
/plugin install agent-team-skills@agent-team-skills
```

Or copy one skill into your personal skills folder:

```bash
git clone https://github.com/kl-autonomous/agent-team-skills.git
```

```bash
cp -R agent-team-skills/skills/owner-desk ~/.claude/skills/
```

## Where they come from

Both came out of a real project: an iPhone app built by a team of five
Claude sessions (a builder, a reviewer, a tester, a feedback reader and a
manager). It went from its first decision to a beta with outside testers in
nine days, with the owner making every decision and writing no app code.
The manager's routine became chief-of-staff, and the board the owner asked
for became owner-desk.

- **owner-desk:** the page in this repo was tested end to end on a demo
  board: posting, waking the agent on an answer, archiving, and answering
  questions, at phone width and in dark mode.
- **chief-of-staff:** written up from daily use on that project. Its setup
  steps haven't been run on a second project yet. Reports are welcome.

## License

MIT. See [LICENSE](LICENSE).
