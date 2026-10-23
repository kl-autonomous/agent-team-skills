---
name: compress-prompt
description: Compress a prompt that will be given to an AI model (a system prompt, a CLAUDE.md, a session's restart or hand-over prompt, a skill's SKILL.md, an agent's instructions) so it is as short as possible while a model reading it behaves the same way. It inventories every load-bearing element first, compresses unevenly (rules lightly, background hard), verifies the result against the inventory, tests both versions where it can, and returns a diff for a human to read, never a silent replacement. Use this whenever someone asks to shorten, compress, tighten, slim, de-duplicate or "make cheaper" a prompt, instructions, a CLAUDE.md or a restart prompt, or asks why a prompt is so long; use it even if they only say "this prompt is too long". Do not use it on records of decisions or on text written in the owner's own words that a team relies on as a record: those are not prompts.
---

# Compress a prompt

A prompt read on every turn is paid on every turn. Shortening it saves a little each time, for the life of the session; but a rule cut by mistake costs far more than it ever saved. So this skill never cuts first: it lists what the prompt does, cuts, then checks the list, and hands a human the diff.

## When not to use it

- **Records are not prompts.** A decisions file, a log, a rule written in the owner's words with a date ("the owner, 23 Oct"), anything a team relies on to know who decided what: leave it. Compressing it loses the provenance that makes it a record. If a file mixes rules and records, compress only the rule parts and keep every attribution and date that remains.
- **A session must not compress its own replacement prompt alone.** A session at the end of its context, rewriting its own rules with no outside reader, is how a rule disappears. The compression is done by another session (a side session, a reviewer) or with a human reading the diff before it replaces anything.

## Work in three passes

### Pass 1: inventory

Before rewriting anything, list every load-bearing element of the original, one short line each:

- the task, the goal, and who the output is for;
- every instruction and constraint, including negations ("never", "don't", "only"), conditions ("if X, do Y"), exceptions, and priorities (which rule wins when two conflict);
- exact literals: numbers, limits, names, file paths, URLs, commands, code, placeholders and variables, quoted strings, required output formats or schemas;
- the examples, and what each shows that the others don't;
- tone, persona, audience and length requirements;
- background facts the task needs;
- attributions and dates that mark a rule as someone's decision.

### Pass 2: compress

Rewrite using only what is in the inventory.

1. **Add nothing:** no new rules, facts, examples or assumptions. Where the original is ambiguous, keep its wording instead of guessing.
2. **Keep meaning exact:** every negation, condition, quantity and ordering stays as it was.
3. **Copy literals verbatim.** Don't abbreviate, round or paraphrase them.
4. **Compress unevenly.**
   - Instructions and constraints: light touch. Each stays a short, complete, unambiguous sentence or bullet, not a telegraphic fragment.
   - Background and rationale: compress hard. Keep a reason only if it tells the model how to handle cases the rules don't cover.
   - Examples: keep the fewest that cover every distinct pattern; drop near-duplicates.
   - Attributions and dates on rules: keep them, shortened to one form ("(the owner, 23 Oct)").
5. **Cut repetition** (keep the clearest version of each rule), filler, pleasantries and hedges. If the original marks a rule as critical, keep that emphasis once.
6. **Merge overlapping rules;** group related ones under short headings if that clarifies the structure.
7. **Put the core task and the most important constraints first.**
8. **No target length.** Stop when further cuts would lose information or create ambiguity.
9. If the original is roughly 1,500 words or more, compress section by section, then do a final pass for duplicates across sections.

### Pass 3: verify, then test

1. Check the result against the inventory item by item, and restore anything missing or weakened.
2. Reread it as if you had never seen the original, and fix anything a model could misread.
3. **Test where you can.** Rereading cannot show that a model behaves the same; only trying can. Pick two or three typical inputs or situations the prompt is for, work through how a model following the original and one following the compressed version would act, and report any difference. Where a real trial is possible (a prompt for a small task, a skill), run both and compare. Where it isn't (a session's restart prompt, which only a real session can try), say so in the flags: the compressed prompt is then "checked, not tried", and the first session that runs on it is its test.

## Output, in this order

1. `<inventory>`: the pass 1 list.
2. `<compressed_prompt>`: the final prompt, ready to paste.
3. `<diff>`: the original against the compressed version, as a unified diff (`diff -u`) or, for prose, old and new side by side per section. The human reads this, not the whole new prompt.
4. `<removed>`: one line per cut, and why.
5. `<flags>`: contradictions or ambiguities in the original that were kept rather than resolved; any cut you weren't sure about; what the test in pass 3 found, or why it couldn't be run.
6. `<length>`: approximate word count, original → compressed, and the tokens that saves per turn (roughly 1.3 tokens per English word, or one per four characters; about one per Chinese character).

The compressed prompt replaces the original only after a human has read the diff and said so. In a team of sessions, that is the owner: the compression goes to them as a proposal (the side-session skill), never straight into the team's files.

## What to expect

A prompt that is already mostly rules and literals has little to give. Measured on six session restart prompts and a CLAUDE.md pitfalls list (10,166 words in all, 2026-10-23): 1–3% shorter each, about 50 tokens a turn, for a diff the owner had to read. The run's value was the inventory and the flags, which named the structural savings (facts that another file also holds, rules that a file read first already carries); those are decisions for the owner, not cuts. Say that plainly in the flags rather than cutting further to show a number.

## Pitfalls

- **Cutting the "why" that carried a rule.** "Never use `Calendar.current`" without "a phone set to the Japanese calendar read 2026 as Reiwa 2026" is a rule a model will argue itself out of. Pass 2 rule 4 keeps the reason when it decides cases.
- **Merging two rules into one that is neither.** "Read PROGRESS.md" and "read only 'Where things stand' of PROGRESS.md" are different rules; the second wins and the first goes.
- **Rounding a literal.** "about 30%" and "30%" are different triggers. Copy.
- **Paraphrasing a quoted string.** A name a session answers to, a tag a tool looks for, a message another session matches on: verbatim, or the match breaks.
- **Trusting the reread.** The model that cut the rule is the model rereading. Pass 3's test, and the human's diff, are what catch it.
