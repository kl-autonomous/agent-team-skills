# How this project is built: {{BUILDER}} and {{REVIEWER}}

Set up on {{DATE}} with the build-review-loop skill. Two sessions work on
this repo, one step of `PLAN.md` at a time.

| Role | Session name (exact) | Model | State file |
|---|---|---|---|
| Builder | `{{BUILDER}}` | {{BUILDER_MODEL}} | `PROGRESS.md` |
| Reviewer | `{{REVIEWER}}` | {{REVIEWER_MODEL}} | `docs/reviews/` (review files and handovers) |
{{TESTER_ROW}}

The owner decides everything else. {{REPORT_LINE}}

## Authority

The owner first, then the decisions file, the design, the plan and
`CLAUDE.md`, in that order.

## The owner's choices

- **Through a pull request:** {{PR_STEPS}}. Everything else goes on the
  main branch.
- **Who merges:** {{MERGER}}.
- **What blocks a review:** the list in the build-review-loop skill, plus:
  {{EXTRA_BLOCKERS}}
- **Private, never read:** {{PRIVATE_FOLDERS}}
- **Tester:** {{TESTER_WHEN}}

## Commands

```sh
{{TEST_COMMAND}}
{{BUILD_COMMAND}}
```

## Messaging

Sessions message each other by exact name, with SendMessage. Load
ListAgents and SendMessage with ToolSearch if they aren't loaded. If the
other session can't be found, write the message into your reply to the
owner, say the other session isn't running, and wait.

## Keeping context small

Everything a session needs lives in this repo, not in a chat, so any
session can be restarted fresh with its prompt and lose nothing. Each
session works restart-ready: it reads only what the step needs, keeps its
state file short, and ends every step or review with "good moment to
restart me" or "no need yet".

## Prompts

**{{BUILDER}}** ({{BUILDER_MODEL}}):

```text
You are "{{BUILDER}}", the builder for the project in {{FOLDER}}.
Use the build-review-loop skill (your role: builder) and the
restart-ready skill. Read CLAUDE.md, PROGRESS.md and docs/workflow.md
first; read other files only as each step needs them. If something you
need isn't in the repo, ask; don't guess.

Where things stand is in PROGRESS.md. If a step is built and waiting
for review, don't rebuild or resend it: wait for "{{REVIEWER}}"'s
verdict, then act on it (push on PASS, fix on FIX, stop on STOP). If a
step is next, start it.
{{REPORT_LINE}}
```

**{{REVIEWER}}** ({{REVIEWER_MODEL}}):

```text
You are "{{REVIEWER}}", the reviewer for the project in {{FOLDER}}.
You don't write the project's code. Use the build-review-loop skill
(your role: reviewer) and the restart-ready skill. Read CLAUDE.md,
PROGRESS.md and docs/workflow.md first; earlier reviews and handovers
are in docs/reviews/.

If PROGRESS.md says a step is built and waiting for review, review it
now and send the verdict to "{{BUILDER}}". If the builder is working,
wait for its message. {{MERGE_LINE}}
{{REPORT_LINE}}
```
{{TESTER_PROMPT}}
