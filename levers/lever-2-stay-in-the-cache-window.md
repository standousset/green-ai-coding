[← Lever 1: Keep the context short](lever-1-keep-the-context-short.md) &emsp; [All levers](../README.md) &emsp; [Lever 3: Use a smaller model →](lever-3-use-a-smaller-model.md)

# Lever 2: Stay in the cache window

> [!IMPORTANT]
> Choose the model and the effort level at the start of a task, do not change them in the middle of it, and compact before a long break.

**Why it matters:** Each request sends the whole conversation again (see
[lever 1](lever-1-keep-the-context-short.md)). Prompt caching lets the provider reuse
its work on the start of the conversation, so it fully processes only what
changed ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching)).
The cache lasts a short time, and a change at the start of the conversation
makes it useless. The items below help you to keep it and to use it.

| Item | Role |
|---|---|
| [2.1 Keep the start of the conversation unchanged](#21-keep-the-start-of-the-conversation-unchanged) | Learn the rule |
| [2.2 Check your cache time before you take a break](#22-check-your-cache-time-before-you-take-a-break) | Learn your cache time |
| [2.3 Set the cache time to 1 hour if you often come back after a break](#23-set-the-cache-time-to-1-hour-if-you-often-come-back-after-a-break) | Set your cache time |
| [2.4 Do not change the model in the middle of a task](#24-do-not-change-the-model-in-the-middle-of-a-task) | Protect the cache: model |
| [2.5 Change the effort level in the middle of a task only on a model that keeps the cache](#25-change-the-effort-level-in-the-middle-of-a-task-only-on-a-model-that-keeps-the-cache) | Protect the cache: effort |
| [2.6 Turn on fast mode at the start of a session](#26-turn-on-fast-mode-at-the-start-of-a-session) | Protect the cache: speed |
| [2.7 Compact while the cache is warm](#27-compact-while-the-cache-is-warm) | Use the warm cache |

## 2.1 Keep the start of the conversation unchanged

**What:** In the middle of a task, do not change what sits at the start of the
conversation, such as the model. Make these changes between tasks.

**Why:** The cache matches the start of each request. The match is exact: a
change anywhere in the start means that everything after it is processed again
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#how-the-cache-is-organized)).
On the Anthropic API, a cache read costs 0.1 times the normal input price. It
costs 0.05 times on Opus 5.5 and Sonnet 5.5, and 0.025 times on Fable 5.1
([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).
This is a price, not a measure of energy. Other providers also cache the start
of the prompt. OpenAI does it automatically for long prompts
([OpenAI docs](https://platform.openai.com/docs/guides/prompt-caching)).

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../images/cache-reuses-the-start-dark.svg">
  <img alt="Two horizontal bars. In a normal turn, the system prompt, the rules file and the earlier messages are read from the cache, and only your new message and the new reply are processed. After a change at the start, for example a new model, the whole conversation is processed again." src="../images/cache-reuses-the-start.svg">
</picture>

**How:** Choose the model, the effort level and the speed at the start of a
task (see 2.4 to 2.6). Before you change another setting in the middle of a
task, check the list of changes that invalidate the cache in the
[Claude Code docs](https://code.claude.com/docs/en/prompt-caching#actions-that-invalidate-the-cache).

**Where:**
- Terminal: `/usage` shows a **Prompt cache** line with the hit ratio and
  whether the cache is warm. It needs Claude Code 2.1.251 or later
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#check-cache-performance)).

**Watch out:** Some changes keep the cache: editing files, changing the
permission mode, using a skill, and spawning a subagent. Editing `CLAUDE.md`
also keeps the cache, but the change applies only after `/clear`, `/compact` or a
restart
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#actions-that-keep-the-cache)).

## 2.2 Check your cache time before you take a break

**What:** Find out if your cache lasts 5 minutes or 1 hour.

**Why:** A break that is longer than the cache time ends the cache. The next
request then reads the full conversation again. With a cache time of 1 hour, the
same break causes no extra cost. The cache starts its time again with each
request that uses it
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#cache-lifetime)).

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../images/cache-time-5-minutes-or-1-hour-dark.svg">
  <img alt="Two timelines that start at your last request. With a cache time of 5 minutes, the cache expires, and after a break of 10 minutes the next request processes the whole conversation again. With a cache time of 1 hour, the cache is still warm after 10 minutes, and the next request reads the conversation from the cache." src="../images/cache-time-5-minutes-or-1-hour.svg">
</picture>

**How:** In Claude Code, the cache time depends on how you pay
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#which-ttl-each-request-gets)):

| How you use Claude Code | Main conversation | Subagents and other requests |
|---|---|---|
| Claude subscription, inside the usage of your plan | 1 hour | 5 minutes |
| Usage credits, API key or cloud provider | 5 minutes | 5 minutes |

When you go over the limit of your plan and Claude Code uses usage credits, the
main conversation drops to 5 minutes. Other requests are the requests outside
the main conversation, such as subagents and compaction.

On the API, 5 minutes is the default. You can ask for 1 hour, but a cache write
costs 2 times the normal input price, against 1.25 times for 5 minutes. A cache
read costs the same for both
([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).

**Where:**
- Terminal: the table above. `/usage` shows whether the cache is warm
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#check-cache-performance)).

## 2.3 Set the cache time to 1 hour if you often come back after a break

**What:** Set `promptCacheTtl` to `1h`, or set the variable
`CLAUDE_CODE_PROMPT_CACHE_TTL=1h`.

**Why:** With an API key, a cloud provider or usage credits, the main
conversation has a cache time of 5 minutes (see 2.2). If you often come back
after a break longer than 5 minutes, the 1-hour cache time avoids the full
re-read. A cache write costs more with 1 hour, so use it only if you often come
back within the hour.

**How:** It needs Claude Code 2.1.242 or later. Each control takes `5m` or `1h`.
For subagents and other requests, the setting is `subagentPromptCacheTtl`
(variable `CLAUDE_CODE_SUBAGENT_PROMPT_CACHE_TTL`). To check the result, run
`claude -p "hello" --output-format json` and read `usage.cache_creation`
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#choose-the-ttl-yourself),
[settings reference](https://code.claude.com/docs/en/settings-reference#promptcachettl)).

**Where:**
- Terminal: the setting and the variable above.

**Watch out:** On a Claude subscription, inside the usage of your plan, the main
conversation already has 1 hour. You do not need this setting there.

## 2.4 Do not change the model in the middle of a task

**What:** Choose the model at the start of a task. Change it between tasks, for
example after `/clear`.

**Why:** Each model has its own cache. When you switch models, the next request
reads the whole conversation with no cache, even if the content is the same
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).
The docs say: "The fewer changes you make mid-task, the higher your cache hit
rate"
([source](https://code.claude.com/docs/en/prompt-caching#how-the-cache-is-organized)).

**How:** Choose the model before the first message. To use a smaller model, see
[lever 3](lever-3-use-a-smaller-model.md).

**Where:**
- Terminal: `/model` asks you to confirm the switch only while the cache is
  warm. After the cache expired, it switches without a question (Claude Code
  2.1.238 or later)
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).
- Desktop app, Code tab: choose the model with the model picker under the prompt
  box. You can change it during the session
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)). The cache docs
  do not describe the desktop app. Each model has its own cache, so a switch
  there can cost the same.

**Watch out:** Some actions are model switches that you may not see. Each
plan-mode toggle with `opusplan` is a switch, and so is an automatic fallback
to another model. A skill that sets a different `model` makes one turn run on
that model
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).

## 2.5 Change the effort level in the middle of a task only on a model that keeps the cache

**What:** Choose the effort level at the start of a task. If you change it in the
middle of a task, check first that your model keeps the cache.

**Why:** On most models, each effort level has its own cache, so a change
reads the whole conversation again. With an API key or a Claude subscription,
the cache stays on Fable 5.1 and later, Opus 5.5, Sonnet 5.5 and Haiku 5.5. On
Amazon Bedrock, Google Cloud's Agent Platform or a Claude apps gateway, a change
of effort restarts the cache on every model
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#changing-effort-level)).

**How:** Choose the effort level at the start (see
[lever 4](lever-4-lower-the-effort.md)). On a model that does not keep the cache,
Claude Code asks you to confirm a change while the cache is warm.

**Where:**
- Terminal: see the rules above.
- Desktop app, Code tab: Cmd+Shift+E opens the effort menu
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)). The cache docs
  do not describe the desktop app.

## 2.6 Turn on fast mode at the start of a session

**What:** If you use fast mode, turn it on at the start of a session, and not in
the middle of a task.

**Why:** Fast mode and standard speed keep separate caches. The first time you
turn fast mode on in a conversation, the next request reads the whole
conversation again, at fast mode rates. Each later toggle also reads uncached
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#turning-on-fast-mode)).

**How:** Turn it on before the first message of the session, or do not use it.

**Where:**
- Terminal: `/fast`.

**Watch out:** If your model does not support fast mode, `/fast` also switches you
to Opus, and this is a model switch (see 2.4). Speed changes that Claude Code
makes by itself, for example after a rate limit, also count as toggles.

## 2.7 Compact while the cache is warm

**What:** Run `/compact` while the cache is warm, before a break that is longer
than the cache time.

**Why:** To make the summary, compacting sends the full conversation once. With a
warm cache, this request reads the conversation from the cache and "costs a
fraction of what the context size suggests". After a break longer than the cache
lifetime, it processes the full history again, with no cache
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#compacting-the-conversation)).

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../images/compact-before-cache-expires-dark.svg">
  <img alt="Two timelines. When you run /compact while the cache is warm, the request reads the conversation from the cache, at a fraction of the cost of its size. When you run it after the cache expired, the request processes the full history again, with no cache." src="../images/compact-before-cache-expires.svg">
</picture>

**How:** Compact at a natural break, before you step away. Your cache time is in
2.2. For the effect of compaction on the context, see
[1.5](lever-1-keep-the-context-short.md#15-compact-when-a-long-task-must-continue).
To abandon a path, `/rewind` is better than `/compact`: it goes back to a part
that is already in the cache (see
[1.4](lever-1-keep-the-context-short.md#14-go-back-with-rewind-when-a-path-did-not-work)).

**Where:**
- Terminal: on the 1-hour cache, Claude Code also compacts an idle conversation
  of 200K tokens or more, about an hour after your last request and before the
  cache expires. The setting is `idleCompaction`, on by default
  ([Claude Code docs](https://code.claude.com/docs/en/settings-reference#idlecompaction)).
- Desktop app, Code tab: type `/compact` in the prompt box
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).

---

[← Lever 1: Keep the context short](lever-1-keep-the-context-short.md) &emsp; [All levers](../README.md) &emsp; [Lever 3: Use a smaller model →](lever-3-use-a-smaller-model.md)
