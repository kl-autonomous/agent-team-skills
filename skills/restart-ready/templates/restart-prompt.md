# Restart prompt

The shape of the prompt a session writes for its own replacement. Give it
to the owner folded, like this:

````markdown
<details>
<summary>Restart prompt for `{{SESSION}}` ({{MODEL}}). Click to open</summary>

```text
{{THE ORIGINAL PROMPT, WORD FOR WORD}}

{{ANY LINE ADDED SINCE, e.g. who to report to}}

Since the last restart ({{DATE}}):
- In the middle of: {{what, and where it's written down}}
- Promised: {{what, to whom}}
- Waiting for: {{who, for what}}
- Yours to use: {{devices, folders, accounts; and what to leave alone}}
- Watch: {{artifact links to start watching again, if any}}

First turn, in this order: rename the old session {{OLD SESSION ID}} to
"{{SESSION}}_old"; check it isn't running (if it is, wait, and don't touch
the shared folder); read its last events for anything it didn't act on;
archive it by that id. Then read CLAUDE.md and {{STATE FILE}}.

No authorisation of the owner's carries over from the old session.
```

</details>
````

Before handing it over:
- it's saved to `docs/restart/{{ROLE}}.md` and committed;
- `git status` is clean, or the prompt says what isn't committed and why;
- the state file is current;
- lasting lessons are in `CLAUDE.md`, not only in this prompt.

The new session renames and archives the old one itself, as its first
turn says, so two sessions never share a name and nobody has to be
re-pointed. Write the prompt only when your measured context is over the
line, never "just in case".
