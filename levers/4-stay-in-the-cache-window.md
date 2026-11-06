# 4 · Stay in the cache window

**In one line:** choose the model and the effort level at the start of a task,
and do not change them in the middle of it.

## Why it matters

Each request sends the full conversation again (see
[lever 3](3-keep-the-context-short.md)). Prompt caching lets the provider reuse
the work already done on the start of the conversation, instead of doing it
again.

- On the Anthropic API, a cache read costs **0.1 times** the normal input price,
  and **0.05 times** on Opus 5.5 and Sonnet 5.5
  ([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).
- The cache stays only for a short time without use. In Claude Code: **1 hour**
  for the main conversation with a Claude subscription, **5 minutes** with an
  API key or a cloud provider. Each use starts the time again
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#cache-lifetime)).
- **Each model has its own cache.** A model change in the middle of a task reads
  the full conversation again, with no cache
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).

Other providers also cache the start of the prompt. OpenAI does it
automatically for long prompts
([OpenAI docs](https://platform.openai.com/docs/guides/prompt-caching)).

## How to apply

**Claude Code**
- Choose the model and the effort at the start of a task. The Claude Code docs
  say: "The fewer changes you make mid-task, the higher your cache hit rate"
  ([source](https://code.claude.com/docs/en/prompt-caching)).
- Change the model between tasks, after `/clear`.
- After a long break, if you do not need the old context, start a new session
  with a short summary. The first request after the cache expires reads the full
  context again.
- With an API key, and if you often take breaks longer than 5 minutes, you can
  set `"promptCacheTtl": "1h"`. A cache write costs more with 1 hour, so do this
  only if you often come back after a break
  ([settings reference](https://code.claude.com/docs/en/settings-reference#promptcachettl)).

**Other assistants and your own scripts:** keep the start of the prompt the
same (rules, instructions, large documents first), and put the parts that change
at the end.

## Copy this

A rule of thumb for each task:

```text
1. /clear
2. Choose the model and the effort.
3. Work without a change until the task is done.
```

## Check it

- `/usage` in Claude Code shows the cache hit ratio of the session and if the
  cache is warm now
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#check-cache-performance)).

## Resources

- [How Claude Code uses prompt caching](https://code.claude.com/docs/en/prompt-caching):
  the actions that keep the cache and the actions that restart it.
- [Prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
  (Anthropic API) and [prompt caching](https://platform.openai.com/docs/guides/prompt-caching)
  (OpenAI API), for your own scripts.

---

Previous: [3 · Keep the context short](3-keep-the-context-short.md) · [All levers](../README.md) · Next: [5 · Cut tools and side work](5-cut-tools-and-side-work.md)
