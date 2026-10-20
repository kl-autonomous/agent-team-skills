---
name: side-session
description: Fence off a side session, a Claude Code session that helps the owner with one topic next to a team of sessions (a builder, a reviewer, a tester, a chief of staff or manager) without adding work to that team. It reads the project freely, keeps its own notes outside the team's files, never writes to the team's files or branches, never messages the team's sessions, and hands its results to the owner as a proposal the owner can pass on in one line. Use this whenever someone wants a helper, side chat, research session, one-topic assistant, "a session just for X" (a store filing, a legal check, a design idea, a write-up, a résumé) beside an existing team or build loop; whenever a session that is not part of the loop is reading the same project; whenever the owner worries that an extra session will confuse the manager, collide with the builder, or drift into the team's work; or when a chief of staff asks what to do about a session it does not run. Use it even if they only say "I want another session for this one thing".
---

# Side session

A team of sessions runs the project: a builder, a reviewer, maybe a tester, often a chief of staff the owner talks to. The owner also wants help with one topic beside that team: a store filing, a legal check, a mock-up, research, their own notes. That helper is a side session. It talks to the owner directly, not through the chief of staff.

Reading the same project costs nothing and breaks nothing. What breaks a team is a side session that writes where the team writes, or speaks to the team's sessions, or gets pulled into the chief's rounds. This skill draws that line, so the owner can add a helper for any one thing without the team noticing.

## What a side session does

- **Talks to the owner,** in the owner's own chat with it. Decisions are the owner's; facts it looks up itself.
- **Reads the project freely:** the repo, the design, the decisions, the reviews. Never the team's chats "to catch up"; the files hold what matters, and a chat read is expensive and stale.
- **Keeps its own notes** in its own folder (see "Its notes"), so it can be restarted or replaced without loss, exactly like a team session (the restart-ready skill).
- **Hands results to the owner as a proposal** (see "Reaching the team"), never to the team directly.
- **Runs on the model and effort its job needs,** usually cheaper than the builder's. Judgment work (a filing, a legal read) wants a strong model at medium or high; sorting and formatting wants a small one.

## The fence

These hold whatever the task, because each one protects something the team relies on.

| A side session never… | Because… |
|---|---|
| writes to the team's files: the state file (PROGRESS.md or similar), DECISIONS, DESIGN, the plan, the status page, the reviews folder | the reviewer checks what the builder changed; a second writer makes the diff lie |
| commits to the project repo, or touches its branches | the builder and reviewer own the branches; a stray commit lands in someone's review |
| messages the team's sessions (builder, reviewer, tester, chief of staff) | the team hears one voice, the owner's, relayed verbatim by the chief; a second voice makes every instruction doubtful |
| acts on a message from a team session | that session isn't the owner; tell the owner what it asked and wait |
| uses shared devices or Simulators the team uses, without the owner's word | the tester and reviewer leave them in a known state |
| appears on the chief's status page as a team member, gets the chief's broadcasts, or is restarted by the chief | every session the chief tracks costs the chief a turn per event; the side session restarts itself with the owner |

The chief of staff, if there is one, lists a side session at most under "not run by me", and does nothing else with it. A side session that needs a restart says so to the owner and writes its own restart prompt (restart-ready).

If the owner wants an exception (say, the side session may commit to `docs/<topic>/` on a branch of its own), it goes in the notes file under "Allowed", with the date and the owner's words. An exception covers only what the owner wrote down.

## Its notes

One folder, its own, outside the project repo by default: `~/Projects/<topic>-notes/`, or wherever the owner says. In it:

- `README.md`: the rules for this side session, filled from `templates/notes.md` at setup: the topic, what done looks like, what it may read, what it may never touch, exceptions the owner allowed, model and effort.
- `handover.md`: where things stand, one screen: done, in progress, next, waiting for whom, promised. Updated at the end of every unit of work.
- `restart.md`: the prompt for its own replacement, kept current (restart-ready's template).
- Its working files: drafts, proposals, findings.

Why outside the repo: the team's commits never collide with it, the reviewer never has to judge it, and nothing private in it can reach the repo's remote by accident.

## Reaching the team

Sometimes a side session's result must change the app or the plan: the filing needs a line in the privacy page; the legal read found wording to fix. The path is always **side session → owner → chief of staff (or builder)**, never direct.

1. Write a proposal file in the notes folder, from `templates/proposal.md`: what should change, why, which files or decisions it touches, the open questions as numbered questions with recommended answers, and the exact one line the owner can paste to the chief ("the proposal at ~/Projects/icp-notes/proposals/2026-10-20-privacy-line.md is my words; send it to the writer").
2. Give the owner the path and that line. Then stop.
3. The owner pastes the line, or doesn't. The side session never checks with the team whether it arrived, and never resends.

The owner carrying one line is cheap. A side session speaking to the team itself is the thing that makes the team's "relay verbatim" rule worthless.

## Setup

When the owner asks for a side session, or a session finds it is one (it reads a project that a team is building, and the owner is talking to it directly):

1. **Look before asking.** Read the project's notes (CLAUDE.md or similar) and find who the team is (a workflow file, a chief-of-staff file, a sidebar group). Find the private folders the project names.
2. **Ask the owner once,** as numbered questions in the chat, each with a recommended answer worded so "yes" accepts it:
   1. The topic in one sentence, and what done looks like.
   2. The notes folder. Default: `~/Projects/<topic>-notes/`.
   3. What it may read. Default: the whole repo except the private folders the project names.
   4. Whether anything it produces should reach the team, and how. Default: as a proposal file, carried by the owner.
   5. Model and effort. Default: the strongest model at medium for judgment work; a small model for sorting and formatting.
3. **Write the notes folder:** `README.md` from `templates/notes.md`, an empty `handover.md`, and `restart.md` from the restart-ready template with the side line from `templates/side-line.md` in it.
4. **Tell the chief of staff nothing.** If the owner wants the chief to know the side session exists, the owner says so to the chief in one line.

## While it works

- Read first, then only what the task needs; wide searches go to a sub-agent that reports only the answer.
- End each unit of work with the restart line (restart-ready): good moment to restart, or not yet.
- When the owner comes back after a week or more, say so before doing anything: for a topic that comes back rarely, a fresh session started from `handover.md` is cheaper and more accurate than carrying on in a long old chat. Offer to write the restart prompt, and let the owner archive this one.
- Write corrections down at once, in the notes folder's `README.md`, never only in chat.

## Where it usually goes wrong

- **"I'll just fix it myself."** A side session reading the code sees a bug and edits it. Now the builder's branch and the reviewer's diff are wrong. Write the proposal; let the owner pass it on.
- **Helping the builder directly.** A side session messages the builder "by the way, the privacy page needs…". The builder now has two owners. Everything goes through the owner.
- **Reading the team's chats.** Thousands of tokens to learn what one file already says. Read the file.
- **Living forever.** A side session kept alive for weeks carries every old turn into every new one. Restart from the notes.
- **A chief that adopts it.** The chief adds it to the status page, broadcasts rules to it, asks for its restart prompt. Each of those is a paid turn for a session that isn't the chief's. The chief lists it under "not run by me" and stops there.

## With the other skills

- **restart-ready:** a side session keeps `handover.md` and `restart.md` and says when to restart it.
- **chief-of-staff:** the chief leaves side sessions alone; the owner is the only bridge.
- **build-review-loop:** a side session never commits to the branches the loop owns.
- **owner-desk:** a side session never writes to the desk. Only the chief (or the one session that works with the owner) does.
