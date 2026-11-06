[← 3 · Keep the context short](3-keep-the-context-short.md) · [All levers](../README.md) · [5 · Cut tools and side work →](5-cut-tools-and-side-work.md)

# 4 · Stay in the cache window

**In one line:** choose the model and the effort level at the start of a task,
and do not change them in the middle of it.

## Why it matters

Each request sends the full conversation again (see
[lever 3](3-keep-the-context-short.md)). Prompt caching lets the provider reuse
the work already done on the start of the conversation, instead of doing it
again.

- On the Anthropic API, a cache read costs **0.1 times** the normal input price.
  It costs **0.05 times** on Opus 5.5 and Sonnet 5.5, and **0.025 times** on
  Fable 5.1 and Mythos 5.1
  ([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).
- The cache stays only for a short time without use. Each use starts the time
  again ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#cache-lifetime)).
  **How long it stays depends on how you pay. See the warning below.**
- **Each model has its own cache.** A model change in the middle of a task reads
  the full conversation again, with no cache
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).

Other providers also cache the start of the prompt. OpenAI does it
automatically for long prompts
([OpenAI docs](https://platform.openai.com/docs/guides/prompt-caching)).

> **Warning: 5 minutes or 1 hour. The difference is large.**
> In Claude Code, the cache time depends on how you pay
> ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#which-ttl-each-request-gets)):
>
> | How you use Claude Code | Main conversation | Subagents |
> |---|---|---|
> | Claude subscription, inside the usage of your plan | **1 hour** | 5 minutes |
> | Claude subscription, after the plan limit (usage credits) | 5 minutes | 5 minutes |
> | API key or cloud provider | 5 minutes | 5 minutes |
>
> With 5 minutes, a short break (a meeting, a coffee, a long test run) can end
> the cache. The next request then reads the full conversation again. With
> 1 hour, the same break costs nothing. So the same habits are cheap for one
> person and expensive for another. Find your case before you use the advice
> below.
>
> On the API, 5 minutes is the default. You can ask for 1 hour, but a cache write
> costs **2 times** the normal input price, against **1.25 times** for 5 minutes.
> A cache read costs the same for both
> ([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).

## How to apply

**Claude Code**
- Choose the model and the effort at the start of a task. The Claude Code docs
  say: "The fewer changes you make mid-task, the higher your cache hit rate"
  ([source](https://code.claude.com/docs/en/prompt-caching)).
- Change the model between tasks, after `/clear`.
- **Compact before you step away, not after.** `/compact` sends the full
  conversation to make a summary. While the cache is warm, this request reads it
  from the cache and costs a fraction of its size. After the cache expires, it
  processes the full history again
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#compacting-the-conversation)).
- After a long break, if you do not need the old context, start a new session
  with `/clear` instead. The first request after the cache expires reads the
  full context again.
- With an API key, a cloud provider or usage credits, and if you often take
  breaks longer than 5 minutes, you can set `"promptCacheTtl": "1h"` (or the
  variable `CLAUDE_CODE_PROMPT_CACHE_TTL=1h`). It needs Claude Code 2.1.242 or
  later. A cache write costs more with 1 hour, so do this only if you often come
  back after a break. For subagents, the setting is `subagentPromptCacheTtl`
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#choose-the-ttl-yourself),
  [settings reference](https://code.claude.com/docs/en/settings-reference#promptcachettl)).

**Other assistants and your own scripts:** keep the start of the prompt the
same (rules, instructions, large documents first), and put the parts that change
at the end.

## Copy this

A rule of thumb for each task:

```text
1. /clear
2. Choose the model and the effort.
3. Work without a change until the task is done.
4. Before a break that is longer than your cache time (5 minutes or 1 hour):
   /compact, while the cache is still warm.
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

[← 3 · Keep the context short](3-keep-the-context-short.md) · [All levers](../README.md) · [5 · Cut tools and side work →](5-cut-tools-and-side-work.md)
