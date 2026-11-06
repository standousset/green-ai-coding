[All levers](../README.md) · [2 · Stay in the cache window →](2-stay-in-the-cache-window.md)

# 1 · Keep the context short

**In one line:** give the assistant only what the task needs, give noisy work to
a subagent, and compact while the cache is warm.

## The assistant sends the whole conversation with every request

Each request sends the full conversation again. Each time the assistant uses a
tool, it sends one more request with the tool results
([Claude Code docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).
Old, unrelated context adds to the cost of each message. In Claude Code,
`/context` shows what fills the context.

## A long context makes the answers less reliable

In a study of 18 models, the results became less reliable as the input got
longer ([Chroma, Context Rot](https://www.trychroma.com/research/context-rot)).
Less reliable answers lead to more retries (see
[lever 5](5-make-fewer-round-trips.md)).

## Compact while the cache is warm

When a long task must continue, `/compact` replaces the old conversation with a
short summary, so each later request is smaller. To make the summary, compacting
sends the full conversation once. With a warm cache, this request reads the
conversation from the cache and "costs a fraction of what the context size
suggests". After a break longer than the cache lifetime, it processes the full
history again, with no cache
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#compacting-the-conversation)).
Compact at a natural break, before you step away (see
[lever 2](2-stay-in-the-cache-window.md)).

## Choose when auto-compaction happens

Claude Code also compacts by itself when the context is full enough. You do not
choose the moment, and it can happen in the middle of a task. The window depends
on the model and can be very large: the docs give about 967K tokens for Haiku
5.5. To compact earlier, use `/autocompact 200k` (or the `autoCompactWindow`
setting). `/autocompact auto` returns to the default. It needs Claude Code
2.1.221 or later
([Claude Code docs](https://code.claude.com/docs/en/model-config#set-the-auto-compact-window)).

## Idle compaction

On the 1-hour cache, Claude Code compacts an idle conversation of 200K tokens or
more, about an hour after your last request and before the cache expires. The
setting is `idleCompaction`, on by default
([Claude Code docs](https://code.claude.com/docs/en/settings-reference#idlecompaction)).

## Go back with `/rewind` when a path did not work

`/rewind` goes back to an earlier message. It keeps the part of the
conversation that is already in the cache
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#rewinding-the-conversation)).

## Give noisy work to a subagent

Test runs, logs and documentation searches fill the context fast. A subagent is
a helper agent with its own context. It does this work and sends back only a
short result
([Claude Code docs](https://code.claude.com/docs/en/costs#delegate-verbose-operations-to-subagents)).

The requests of a subagent use energy too. For simple work such as running
tests, give it a small model, for example `model: haiku` in the subagent file
([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model);
see also [lever 3](3-use-a-smaller-model.md)).

A subagent does not use the warm cache of the main conversation. It starts its
own cache, which lasts 5 minutes by default
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#which-ttl-each-request-gets)).
To write a subagent, see the
[Claude Code docs](https://code.claude.com/docs/en/sub-agents).

## Send only what the task needs

Give only the part of a log or a file that matters. Point to the files:
"change `src/auth/login.ts`" is better than "find the login code". Ask for short
answers, for example "Be brief. Show only the code that changes."

Keep `CLAUDE.md` or `AGENTS.md` short, because it is part of every request.
[AGENTS.md](https://agents.md/) is a shared format for the rules file of coding
assistants.

## Disconnect the tools that you do not use

Tool servers (MCP servers) add their tool names and instructions to the
context. Claude Code loads the full tool definitions only when it needs them, so
the cost is small. `/mcp` lists the servers, and you can disable the ones that
you do not use. Command-line tools such as `gh`, `aws` or `gcloud` add no tool
list at all
([Claude Code docs](https://code.claude.com/docs/en/costs#reduce-mcp-server-overhead)).
In other assistants, disconnect the extensions and tool servers that you do not
use for the current project.

---

[All levers](../README.md) · [2 · Stay in the cache window →](2-stay-in-the-cache-window.md)
