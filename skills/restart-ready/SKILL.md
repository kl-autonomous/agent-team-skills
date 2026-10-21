---
name: restart-ready
description: Keep a Claude Code session ready to be thrown away and restarted fresh at any moment without losing anything. Everything it knows lives in short .md files in the repo; it reads only what each step needs; it ends each unit of work by saying whether now is a good moment to restart it; and on request it writes the full prompt for its own replacement. Use this for any long-running or repeated work in one project, whenever a chat is getting long, slow or forgetful, when the user asks to compact, reset, save context, hand over or "restart fresh", when a session is about to be replaced, or when several sessions share a project. Use it even when the user only says the conversation is getting long.
---

# Restart-ready

A chat is short-term memory; files are long-term memory. A long chat gets slow, expensive and forgetful, and whatever lives only in it is lost when it ends. If everything that matters is in files, a fresh session started with the same prompt reads them and carries on, so the session itself becomes disposable. That's the point of this skill: restart whenever the context grows, and lose nothing.

## The files

Keep a small set, each with one job. Use the project's existing names where they exist.

| File | What it holds | Who reads it |
|---|---|---|
| **Your state file**, e.g. `PROGRESS.md` for a builder or `docs/handover/<role>.md` for others | Where things stand for your role: done, in progress, next, waiting for whom, promises, what's yours | You, first thing, every time |
| **The log**, e.g. `docs/log/<role>.md` | The history: what happened, step by step | Only when a question needs the past |
| **The project notes** (`CLAUDE.md`) | Rules every session follows, and "pitfalls already hit" | Every session, first thing |
| **The decisions** (`DECISIONS.md`, or wherever the project keeps them) | The owner's decisions, dated, in their words where it matters | When a step touches one |
| **The prompts** (e.g. `docs/workflow.md`) | Each session's starting prompt, word for word | When restarting a session |

Start a state file from `templates/state.md`.

**Keep the state file short.** Its top section, "Where things stand", should fit on one screen. Move finished history to the log as you go. A state file that grows to a thousand lines makes every fresh session spend its context reading the past, which is the problem this skill exists to solve.

## While you work

1. **Read first, then only what the step needs.** At the start: the project notes, then the top of your state file, then the files the current step touches. Don't read the whole repo or the log "to get context".
2. **Send wide searches to a sub-agent** and have it report only the answer. Its reading never enters your context.
3. **Keep big outputs out of the chat.** Save long logs and test output to files and read the part that matters. Look at screenshots at full size, but send them scaled down.
4. **Update your state file at the end of each unit of work,** before you report it done. Commit it with the work when you can.
5. **Write corrections down at once.** When the owner corrects you or decides something, record it straight away: a rule in the project notes, a decision in the decisions file, with the date and their words. A correction left in chat dies with the session, and the next session repeats the mistake.
6. **Never keep a promise only in chat.** "I'll check that after the merge" goes under "Promised" in your state file.
7. **Group tool calls.** Every tool call is a turn, and every turn re-reads your whole context. Run independent calls together, and never spend a call on a routine line: a separate `date` before every message once cost one session 280 turns.
8. **Know which files are read whole.** The state file is read in full at every start, so it stays one screen. Lookup files, such as decisions by number, may be long, as long as every entry has a key to find it by.

## End each unit of work with the restart line

Finish every step, review or report with one line on your context, with its number:

> Context: 41%, after three reviews. Good moment to restart me: yes. Everything is committed and the state file is current.

or

> Context: 12%. No need to restart yet.

Read the number from the app's usage tool (in the Claude desktop app, `get_usage` on "self"), not from a feeling. Say yes only when both hold:

1. **Nothing lives only in the chat:** everything is committed and the state file is current.
2. **The chat is big or has gone wrong:** the context is over about 30% of the window (about 300,000 tokens on a 1M window), or you show one of the signs below (a corrected mistake repeated, files re-read, a decision lost).

Being idle is not a reason. An idle session with a small context costs nothing to keep, and a restart costs a fresh read of every file: say "no need yet". Say no while you're in the middle of something that lives only in chat; finish it or write it down first. The moment just after a unit of work is when a restart costs least, but only if one is due.

## Writing your own restart prompt

When you're asked for it, or after you said yes and the owner agrees:

1. **Finish or park the current work.** Commit it, and bring the state file up to date. Move lasting lessons into the project notes.
2. **Write the prompt** from `templates/restart-prompt.md`:
   - your original prompt, word for word, from the file that keeps it;
   - any lines added since (such as a line saying who to report to);
   - a short "Since the last restart" section with only what isn't in the files yet: what you're in the middle of, what you promised, who is waiting for what, and which shared things (devices, folders, accounts) are yours to use.
3. **Check `git status`.** Nothing important should be uncommitted. If something is, say what and why.
4. **Save it to a file too**, such as `docs/restart/<role>.md`, and commit it, so it survives even if the chat that delivers it doesn't.
5. **Hand it over** in a collapsible `<details>` block, with your exact session name and model. Remind the owner to archive the old session and rename it (`builder_old2`), so two sessions never share a name. If you watch an artifact (such as an owner desk), put its link in the prompt; the new session must start watching it again.

A good restart prompt is short: it points at the files, which hold the detail.

## Signs a restart is due

- You repeat a mistake that was already corrected.
- You re-read files you read earlier in the same chat.
- You lose track of a decision or of who is waiting for what.
- Your summaries of earlier work get vague.
- Replies slow down, or the context is over about 30% of the window.

Any of these means: finish the unit, update the files, and say it's a good moment to restart you.

## What not to do

- **Don't copy the chat into the state file.** Write the state (where things are), not the story (how they got there). The story goes in the log, briefly.
- **Don't let one file do every job.** Status, history, rules and decisions grow at different speeds; mixed together they bury each other.
- **Don't wait to be told.** Say when it's a good moment to restart. The owner can't see your context; you can.
- **Don't ask for a restart because you're idle.** A chief that acts on "yes" restarts you for nothing; one session was restarted three times in a day at about 10% context.
