[← 1 · Keep the context short](1-keep-the-context-short.md) · [All levers](../README.md) · [3 · Use a smaller model →](3-use-a-smaller-model.md)

# 2 · Stay in the cache window

**In one line:** choose the model and the effort level at the start of a task,
do not change the model in the middle of it, and compact before a long break.

## Prompt caching reuses the start of the conversation

Each request sends the full conversation again (see
[lever 1](1-keep-the-context-short.md)). Prompt caching lets the provider reuse
the work already done on the start of the conversation, instead of doing it
again.

On the Anthropic API, a cache read costs 0.1 times the normal input price. It
costs 0.05 times on Opus 5.5 and Sonnet 5.5, and 0.025 times on Fable 5.1
([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).
Other providers also cache the start of the prompt. OpenAI does it
automatically for long prompts
([OpenAI docs](https://platform.openai.com/docs/guides/prompt-caching)).

## The cache lasts 5 minutes or 1 hour

The cache stays only for a short time without use. Each use starts the time
again ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#cache-lifetime)).
In Claude Code, how long it stays depends on how you pay
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#which-ttl-each-request-gets)):

| How you use Claude Code | Main conversation | Subagents |
|---|---|---|
| Claude subscription, inside the usage of your plan | 1 hour | 5 minutes |
| Claude subscription, after the plan limit (usage credits) | 5 minutes | 5 minutes |
| API key or cloud provider | 5 minutes | 5 minutes |

With 5 minutes, a short break (a meeting, a coffee, a long test run) can end the
cache. The next request then reads the full conversation again. With 1 hour, the
same break causes no extra cost. Check which case is yours before you use the
advice below.

On the API, 5 minutes is the default. You can ask for 1 hour, but a cache write
costs 2 times the normal input price, against 1.25 times for 5 minutes. A cache
read costs the same for both
([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).

## Set the cache time yourself

If you use an API key, a cloud provider or usage credits, and you often come
back after a break longer than 5 minutes, you can set `promptCacheTtl` to `1h`
(or the variable `CLAUDE_CODE_PROMPT_CACHE_TTL=1h`). It needs Claude Code
2.1.242 or later. A cache write costs more with 1 hour, so use it only if you
often come back within the hour. For subagents, the setting is
`subagentPromptCacheTtl`
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#choose-the-ttl-yourself),
[settings reference](https://code.claude.com/docs/en/settings-reference#promptcachettl)).

## Do not change the model in the middle of a task

Each model has its own cache. A model change in the middle of a task reads the
full conversation again, with no cache
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).
Choose the model at the start of a task, and change it between tasks, after
`/clear`. The Claude Code docs say: "The fewer changes you make mid-task, the
higher your cache hit rate"
([source](https://code.claude.com/docs/en/prompt-caching)).

## A change of effort may keep the cache

A change of effort keeps the cache on Opus 5.5, Sonnet 5.5, Haiku 5.5 and
Fable 5.1 if you use an API key or a Claude subscription. On other models, and
on Amazon Bedrock and Google Cloud, it restarts the cache
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#changing-effort-level)).
See [lever 4](4-lower-the-effort.md).

## Compact before you step away

`/compact` sends the full conversation to make a summary. While the cache is
warm, this request reads it from the cache and costs a fraction of its size.
After the cache expires, it processes the full history again
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#compacting-the-conversation)).
So compact before a break that is longer than your cache time (5 minutes or
1 hour). See [lever 1](1-keep-the-context-short.md).

## Start a new session after a long break

After a long break, if you do not need the old context, start a new session with
`/clear`. The first request after the cache expires reads the full context
again.

## Keep the start of the prompt the same in your own scripts

In other assistants and in your own scripts, put the rules, the instructions and
the large documents first, and the parts that change at the end. The provider
can then reuse the start of the prompt from its cache.

---

[← 1 · Keep the context short](1-keep-the-context-short.md) · [All levers](../README.md) · [3 · Use a smaller model →](3-use-a-smaller-model.md)
