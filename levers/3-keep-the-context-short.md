[← 2 · Use a smaller model](2-use-a-smaller-model.md) · [All levers](../README.md) · [4 · Stay in the cache window →](4-stay-in-the-cache-window.md)

# 3 · Keep the context short

**In one line:** start a new session for each task, and give the assistant only
what the task needs.

## Why it matters

- The assistant sends the **full conversation again with every request**. Each
  time it uses a tool, it sends one more request with the tool results
  ([Claude Code docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).
  Old, unrelated context costs something with each message.
- A long context also makes the answers less reliable. In a study of 18 models,
  the results became less reliable as the input got longer
  ([Chroma, Context Rot](https://www.trychroma.com/research/context-rot)).
  Less reliable answers mean more retries (see [lever 6](6-make-fewer-round-trips.md)).

## How to apply

**Claude Code**
- New task: `/clear`. It costs nothing.
- Long task that you want to continue: `/compact`, at a natural break, **while
  the cache is still warm**. To make the summary, compacting sends the full
  conversation again. With a warm cache, this request reads the conversation
  from the cache and "costs a fraction of what the context size suggests". After
  a break longer than the cache lifetime, it processes the full history again,
  with no cache ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#compacting-the-conversation)).
  So compact before you step away, not when you come back. See
  [lever 4](4-stay-in-the-cache-window.md).
- To drop a path that did not work, use `/rewind` to go back to an earlier
  message. It keeps the part of the conversation that is already in the cache.
- See what fills the context: `/context`.
- Keep `CLAUDE.md` or `AGENTS.md` short. It is part of every request.

**All assistants**
- Give only the part of a log or a file that matters, not the full file.
- Point to the files: "change `src/auth/login.ts`" is better than "find the
  login code".
- Ask for short answers (see the text to copy below).

**Teams:** review the shared rules file like code. Remove the rules that nobody
needs.

## Copy this

Add these lines to your `CLAUDE.md`, `AGENTS.md` or prompt:

```text
- Be brief. Show only the code that changes. No summary at the end unless I ask.
- Read only the files that the task needs. Ask before you read the whole project.
- Summarise long outputs (tests, logs, searches): give the result, not the full text.
```

At the end of a long session, before `/clear`:

```text
Summarise this session in 10 lines: decisions, open points, files changed. I will start a new session with this summary.
```

## Check it

- `/context` shows how much space each part uses (rules, tools, conversation).
- [ccusage](https://github.com/ryoppippi/ccusage) (community tool) reads the
  local Claude Code logs and shows the tokens of each day and session.

## Resources

- [Manage costs effectively](https://code.claude.com/docs/en/costs) (Claude
  Code docs): the official list of ways to use fewer tokens.
- [Context Rot](https://www.trychroma.com/research/context-rot) (Chroma):
  how the length of the input changes the results of 18 models.
- [AGENTS.md](https://agents.md/): a shared format for the rules file of coding
  assistants.

---

[← 2 · Use a smaller model](2-use-a-smaller-model.md) · [All levers](../README.md) · [4 · Stay in the cache window →](4-stay-in-the-cache-window.md)
