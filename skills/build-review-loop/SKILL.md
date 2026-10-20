---
name: build-review-loop
description: Run a project with two Claude Code sessions that check each other. A builder builds one step of the plan at a time, with tests and evidence; an independent reviewer checks the exact commit in its own copy, re-runs everything, and answers PASS, FIX or STOP against a short list of what blocks. An optional tester makes adversarial passes. Both sessions keep their state in files, so either can be restarted at any time. Use this when someone wants a writer and a reviewer, a second session to review the first one's work, AI code review that doesn't just trust the author, a step-by-step build with verdicts, or a "builder/checker" or "maker/checker" setup; when a project holds data that must not be lost; or when one session's own tests keep passing while bugs get through. Use it even if they only say they want their AI's work checked.
---

# Build-review loop

One session builds; another, independent session checks. An agent that grades its own work misses what a second agent catches. In the project this comes from, the reviewer sent work back 7 times with 12 blocking problems, among them a "fix" whose new test also passed on the old, broken code. The loop is a contract between the two roles, so both read this file, and each then reads its own role file.

## The roles

| Role | Reads | Does | Never |
|---|---|---|---|
| **Builder** | `roles/builder.md` | builds one step of the plan, with tests and evidence; acts on the verdict | reviews its own work; starts the next step before the verdict |
| **Reviewer** | `roles/reviewer.md` | checks the exact commit in its own copy; answers PASS, FIX or STOP; writes a review file | edits the code, tests or design; pushes to the main branch |
| **Tester** (optional) | `roles/tester.md` | makes adversarial passes, trying to break what's built | edits, commits or pushes |

Use a different model for the reviewer than for the builder where you can; a different model misses different things. This drifts: when one model's usage runs out, the reviewer gets moved to the builder's model. Put it back when the usage resets. Every role works restart-ready (see the restart-ready skill): its state lives in files, and it ends each unit of work by saying whether it's a good moment to restart it.

## Setup

1. **Look before asking.** Read the project's notes, README and plan, and find how to run the tests and the build. Don't ask the owner what the repo already says.
2. **Ask the owner once,** as numbered questions in the chat. Give each a recommended answer, worded so "yes" accepts it:
   1. **Session names and models.** Default: `builder` on the strongest model, `reviewer` on a different one. Names must be unique among running sessions.
   2. **What goes through a pull request.** Default: steps that touch stored data, security or money; everything else goes on the main branch.
   3. **Who merges a pull request.** Default: the reviewer, after its own PASS, as a plain merge.
   4. **What blocks a review.** Default: the list under "What blocks", plus anything special to this project ("imported files must give the same totals as the old app").
   5. **Private folders,** which no session may read (real records, personal files). Default: none.
   6. **A tester for risky parts** (sync, payments, imports). Default: only when a step is risky.
3. **Write `docs/workflow.md`** from `templates/workflow.md`, filled in. If the project has no plan, add `PLAN.md` from `templates/plan.md`. The builder's first step is then drafting the plan for the owner to approve.
4. **Hand over.** Give the owner each session's exact name, model and prompt, from the workflow file, each folded in a `<details>` block. In a git repo, ask before committing. Commit only these files.

## The loop

1. **The builder finishes a step:** the code with tests, its state file updated, any new decision recorded, plain commits. It puts a step on a branch with a pull request if the workflow says that kind of step goes through one.
2. **The builder messages the reviewer** (by name, with SendMessage) with the step, the commit range and the evidence: the commands it ran and their results, plus screenshot paths. Then it ends its turn and waits.
3. **The reviewer checks it** (see `roles/reviewer.md`). It writes `docs/reviews/<date>-<step>.md`, commits only that file on the branch under review without pushing, and messages the builder one verdict:
   - **PASS.** The builder pushes. For a pull request, the reviewer merges after its PASS if the workflow allows it; the builder waits until it's merged, updates its branch, and goes on.
   - **FIX,** with a numbered list of blocking findings. The builder fixes only those and sends the step back.
   - **STOP.** Something needs the owner, or the same problem came back twice. The builder stops and writes the owner a summary.
4. **If they disagree twice about the same finding,** the builder stops and leaves it for the owner. Neither side argues a third round.
5. **If the reviewer's model refuses to review something,** the reviewer replies REFUSED, with the step and what it couldn't review, and waits. The builder (or a chief of staff) switches the reviewer's model for that one review (`set_session_model`, loaded with ToolSearch), asks for the review again, and switches back afterwards. If the switch isn't possible, ask the owner to change the model in the reviewer's model menu.

## What blocks

Only these send work back:

- **Data safety:** records could be lost, overwritten or corrupted, or a safety rule has no test.
- **Privacy:** something leaves the device or the system without the owner's OK, or outside code was added without it.
- **Wrong results** under the project's rules.
- **A broken build or failing tests.**
- **Contradicting a recorded decision** or the design.
- **Scope creep** beyond the step.
- **Records not updated:** the state file, or the decisions file for a new decision.
- Anything the owner added in setup.

Style isn't a finding. Don't invent problems to have something to report. A reviewer that always finds something trains everyone to ignore it.

## The owner

Decisions are the owner's; facts are the sessions'. Ask decisions as numbered questions, each with a recommended answer, worded so "yes" accepts it. Look facts up yourself. Nothing is installed, bought, published or deleted without the owner's word in chat.

- With the chief-of-staff skill, everything for the owner goes to the chief of staff instead, which relays the owner's answers word for word.
- With the owner-desk skill, the owner may answer on the desk.
