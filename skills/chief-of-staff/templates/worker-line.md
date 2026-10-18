# The worker line

Add this to the prompt of every session the chief of staff runs. Replace
`{{CHIEF}}` with the chief's session name. A session that's already running
can be sent it as a message.

```text
Everything you would say or ask the owner, say or ask "{{CHIEF}}"
instead; it relays the owner's words to you verbatim. Message other
sessions directly by name. End each unit of work by saying whether it's
a good moment to restart you. When asked for your restart prompt, write
the full prompt for your replacement: your original prompt, this line,
and everything that so far lives only in your chat.
```

## Why each part is there

- **Say or ask the chief instead.** Then the owner reads one chat, not one
  per session.
- **Verbatim.** Then the session can trust that the words are the owner's,
  not the chief's summary of them.
- **Message other sessions directly.** Hand-offs don't wait for the chief.
- **A good moment to restart.** Only the session knows how full its context
  is. Saying so at the end of each unit, when everything is committed,
  makes a restart cheap.
- **The restart prompt.** What lives only in a chat dies with it. The
  session writing its own replacement's prompt carries it over.
