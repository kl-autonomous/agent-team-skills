# build-review-loop

A Claude Code skill for two sessions that check each other: a builder
builds one step at a time, and an independent reviewer checks each step
before the next one starts.

> **What it needs:** the Claude desktop app (Code tab), where sessions can
> message each other, and a git repo with tests you can run.

## The problem

An AI that grades its own work misses what a second one catches. Its tests
check what it thought of, and its "all tests pass" can be true while the
bug is still there.

In the project this skill comes from, the builder fixed a sync bug and
added a test. The reviewer found two problems. The fix could still delete a
record from both devices, and the new test also passed on the old, broken
code, so it proved nothing. Neither showed up in the builder's own report.

## How the loop works

1. The **builder** builds one step of the plan, with tests and evidence,
   updates its state file, commits, and asks the reviewer for a review.
   Then it waits.
2. The **reviewer** checks out the exact commit in its own copy and
   re-runs everything. It checks that new tests fail on the old code, and
   gives one verdict:
   - **PASS:** the builder pushes, and the next step starts;
   - **FIX:** a numbered list of blocking problems; the builder fixes
     only those;
   - **STOP:** something needs you.
3. Only a short list of problems blocks: lost or corrupted data, privacy,
   wrong results, a broken build, contradicting a decision, scope creep,
   records not updated. Style doesn't block. A reviewer that always finds
   something trains everyone to ignore it.
4. If the two disagree twice about the same thing, it comes to you.
5. An optional **tester** makes adversarial passes on the risky parts
   (sync, imports, payments) and writes a report. It never edits anything.

Every role works [restart-ready](../restart-ready/): its state lives in
files, so any session can be restarted at any time without losing work.

## Where it comes from

A real iPhone app built over nine days by a builder, a reviewer on a
different model, and a tester:
- 28 review files;
- 7 reviews sent work back, with 12 blocking problems, all fixed before
  merge;
- two test passes found 13 problems, among them a "Keep" button that could
  undo days of edits on every device.

## Install

```bash
git clone https://github.com/kl-autonomous/agent-team-skills.git
```

```bash
cp -R agent-team-skills/skills/build-review-loop agent-team-skills/skills/restart-ready ~/.claude/skills/
```

## Use it

1. In your project, say: "set up a build-review loop".
2. It reads the project, then asks you a few numbered questions: names and
   models, what goes through a pull request, who merges, and what else
   should block a review.
3. It writes `docs/workflow.md` with both sessions' prompts, plus a plan
   if there isn't one.
4. You create the two sessions with those prompts. From then on, the
   builder builds and the reviewer reviews.

## Works well with

- [chief-of-staff](../chief-of-staff/): one session that runs the loop
  for you and talks to you instead of both.
- [owner-desk](../owner-desk/): a board for the decisions the loop needs
  from you.

## License

MIT
