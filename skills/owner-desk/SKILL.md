---
name: owner-desk
description: Give the human owner a live board, a Claude artifact with its own small database, for everything only they can decide or do. Each decision or task gets its own tab, with a recommended answer, answer buttons, steps to tick, text to copy and files; the owner answers from their phone whenever they like, the answer wakes the agent through a comment, and the agent acts and archives the item. Use this whenever an agent has several open questions or owner-only tasks (approvals, uploads, sign-ins, purchases, account changes), when the owner says questions get lost or buried in chat, when they ask for a dashboard, inbox, to-do board, decision board or "desk" for what's waiting on them, or when a chief-of-staff or manager session needs a home for its "waiting for the owner" list.
---

# Owner desk

Questions in chat get buried, and the owner has to answer when the agent asks, not when they have time. The desk moves them out of the chat into a board: a Needs you tab with every open item (each also gets a tab of its own), a Decided tab for answered ones, a Later tab for what can wait, and an Archive for what's done. On a phone or iPad, the owner swipes sideways to move between tabs. The owner answers there; you read the answers, act, and archive.

## What this needs

- **Claude artifacts with the `db` and `comments` capabilities**, on claude.ai or in the Claude desktop app. Add `assets` only if items will carry attached files.
- **Tools:**
  - `Artifact`, to publish the board and upload files;
  - `ArtifactData`, to write and read items;
  - `ArtifactComments`, to watch the board and answer the comments that wake you.

  Load them with ToolSearch when they're deferred.
- **A main-loop session.** Only a main-loop session can hold a watch; a subagent can't.

Without these, say so and keep the questions in chat as a numbered list. Don't pretend there's a board.

## Who writes to it

One session only:
- the chief of staff, if there is one (see the chief-of-staff skill);
- otherwise, the session that works with the owner.

Two writers would number items twice and archive each other's work.

## Create the board

1. Fill in the template, `assets/desk.html`. It has three placeholders:
   - `{{TITLE}}`: the board's name, two to four words, such as "Recipe App Desk";
   - `{{EYEBROW}}`: a small line above the title, such as "Recipe app · from the chief of staff";
   - `{{AGENT}}`: who reads the answers, as it reads mid-sentence: "the chief of staff", or "Claude". Plain words only, with no quotes.

   For example:
   ```bash
   sed -e 's/{{TITLE}}/Recipe App Desk/g' -e 's/{{EYEBROW}}/Recipe app · from the chief of staff/g' -e 's/{{AGENT}}/the chief of staff/g' <skill>/assets/desk.html > desk.html
   ```
2. Publish it with the `Artifact` tool:
   - `capabilities: {"db": {}, "user": {}, "comments": {}}`, plus `"assets": {}` if items will carry files;
   - `icon: "inbox"`;
   - a one-line `description`.

   Publishing also starts this session watching the board.
3. Check it once: `ArtifactData` `list` on the collection `items`. It's empty, and the page says "Nothing needs you."
4. Give the owner the link, and say the board is private to them.
5. Write the link into a file in the project (the status page, or the project's notes), so a fresh session can find it.

Opened outside Claude, in a plain browser, the page shows made-up example items with a note saying so. That's for previews and screenshots; it never writes anything.

## Be woken by answers

A database write doesn't wake you; a comment sent to Claude does. When the owner answers, marks a task done or asks a question, the page sends a short comment into the item's own thread, for example "№ 12 · Open the beta this Friday? Answer: Yes". That comment wakes the session watching the board.

- **Publishing starts the watch,** but it's only a watch once `ArtifactComments` `watch` (with no `url`) lists the board as connected, with auto-replies armed.
- **A fresh session must watch again,** with `ArtifactComments` `watch` and the board's `url`. Auto-replies arm only when the owner gave that link in their own message, so the link goes in the restart prompt the owner pastes.
- The page tells the owner which case they're in: "…is told the moment you answer", or "Saved here; … reads it at its next check".
- **If comments can't reach you** (they're off, or no session is watching), fall back on these:
  - read the desk at the start of every turn;
  - ask the owner to say "desk" in chat after answering;
  - offer a scheduled check, only if the owner wants one.

## An item

Each item is one document in the collection `items`. Its document id is its handle: use `q<number>` for decisions and `t-<word>` for tasks. Use plain words throughout, since the owner may read it on a phone.

| Field | Written by | What it holds |
|---|---|---|
| `kind` | you | `"decision"` or `"task"` |
| `status` | both | `"open"`; the page sets `"answered"`; you set `"archived"` |
| `later` | you | `true` parks it on the Later tab |
| `num` | you | a running number, the same one you use in chat; never reused |
| `title` | you | the question itself, answerable with the choices, or the task as an action |
| `short` | you | the tab's label, two or three words |
| `body`, `bullets` | you | short paragraphs and points: why it matters, what it affects |
| `recommendation` | you | your answer first, then the reason, in one or two sentences |
| `choices` | you | two to four answers; defaults to Yes / No |
| `steps`, `stepsDone` | you; page | a task's steps, and the ones ticked (indexes) |
| `copies` | you | `[{label, text}]`: text the owner will paste somewhere |
| `links` | you | `[{label, url, note}]`: guides, pages, attached files |
| `from` | you | which session asked |
| `priority` | you | 1 first; sorts the open items |
| `created` | you | when you posted it (ISO time) |
| `answer` | page | `{choice, note, at}` |
| `asks` | page; you | `[{q, at, via, a, aAt}]`: the owner's questions about the item; you fill in `a` and `aAt` |
| `thread` | page | the comment thread the page uses for this item; leave it alone |
| `outcome`, `archived` | you | what happened, in one line; when you archived it |

`assets/example-items.json` holds six made-up items, as `ArtifactData` `batch` writes. Use them to try a board, never on the owner's real one.

## Post items

- Add new items with `ArtifactData`. Use `set` for a new document, and `batch` for more than two. Never republish the page to change items.
- Then say in chat what you added, with a link to each item's tab: `<board url>#<doc id>`.
- If you also work from a chat list of open questions (as the chief of staff does), keep both the same: the same number, the same words.
- Write each item so it stands on its own: what is being decided, what changes either way, and what you recommend.
- A task lists the exact steps, the text to paste in `copies`, and the guide in `links`.
- **To attach a file:** read the whole file first, then upload it with `Artifact` (`url` of the board, `asset: true`) and link it as `/_blob/<asset id>`.
- **Never put passwords, keys or private records in an item.** A task that needs a secret says where to type it, never what it is.

## Read, act, archive

When a comment wakes you, or at the start of a turn:

1. `ArtifactData` `query` on `items` with `where: [["status", "==", "answered"]]`. Read each item's `answer` and any new `asks`.
2. **Act on the answer.** With a chief of staff, relay the owner's choice and note verbatim to the session that asked.
   - The comment and the item normally agree.
   - If the item isn't saved as answered, but the comment carries an answer, the comment is the owner's newer word. Write it into the item yourself.
3. **Archive the item:** `update` it with `status: "archived"`, a one-line `outcome` ("The builder moved the beta to Friday"), and `archived` (the time). Pin every write with `if_version`, so an answer the owner changes meanwhile isn't overwritten.
4. **Answer each open entry in `asks`** on the same item. Send the whole `asks` array back, with `a` and `aAt` filled in, pinned with `if_version`.
5. **Reply in the comment thread** with one line ("Got it: moving the beta to Friday"), then resolve the thread (`ArtifactComments` `reply`, then `resolve`).

Park an item with `later: true`, and bring it back with `later: false`. A decided item can stay on Later until it's time to act on it. Archive instead of deleting: the Archive is the record of what was decided. Delete only on the owner's word.

## Change the page

- Republish the page only to change the page itself. Pass the same capabilities, or omit `capabilities` to keep them.
- If another session published the board, read it first (`Artifact` `read`) and build on what comes back.
- The items live in the database, so they survive a republish.
