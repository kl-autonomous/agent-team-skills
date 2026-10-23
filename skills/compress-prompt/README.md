# compress-prompt

Shortens a prompt an AI model will read, without changing how the model behaves: it lists every load-bearing element first, cuts unevenly (rules lightly, background hard), checks the result against the list, tests both versions where it can, and gives you a diff to read.

It is for prompts: system prompts, a CLAUDE.md, a session's restart prompt, a skill's instructions. It is not for records: a decisions file, a log, rules written in the owner's own words with a date. Those lose their provenance when compressed.

A session never compresses its own replacement prompt alone; another session does it, and a human reads the diff before anything is replaced.

See [SKILL.md](SKILL.md).
