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

Start by reading CLAUDE.md and {{STATE FILE}}.
```

</details>
````

Before handing it over:
- it's saved to `docs/restart/{{ROLE}}.md` and committed;
- `git status` is clean, or the prompt says what isn't committed and why;
- the state file is current;
- lasting lessons are in `CLAUDE.md`, not only in this prompt.

After the owner starts the new session: archive the old one and rename it
(`{{SESSION}}_old1`), so two sessions never share a name.
