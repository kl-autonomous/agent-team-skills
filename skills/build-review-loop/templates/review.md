# Review of {{STEP}}

Date: {{DATE}}. Reviewer `{{REVIEWER}}` ({{MODEL}}). Commits
{{RANGE}}{{PR}}. Checked in a worktree at `{{COMMIT}}`, with its own build
folder.

## Verdict: {{PASS | FIX | STOP}}

{{For FIX: the numbered blocking findings. For each: what's wrong, how to
reproduce it, and why it blocks (data safety, privacy, wrong result,
broken build, contradicts a decision, scope creep, records not updated).
For STOP: what needs the owner.}}

1. **{{Title}}.** {{What, how to reproduce, why it blocks.}}

## What was checked

| Check | Result |
|---|---|
| The whole test suite | {{counts, all pass?}} |
| The build | {{builds?}} |
| New tests fail on the code before the change | {{yes / not applicable}} |
| The code, read | {{what was looked at, what was found}} |
| On screen | {{modes and sizes, screenshot paths}} |
| Records (state file, decisions) | {{current?}} |

## Notes

Things that don't block: style, naming, ideas for later. Leave this
section out if there's nothing to say.

Context: {{percent}}%. {{Good moment to restart me / No need yet}}.
