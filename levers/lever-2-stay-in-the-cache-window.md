[← Lever 1: Keep the context short](lever-1-keep-the-context-short.md) &emsp; [All levers](../README.md) &emsp; [Lever 3: Use a smaller model →](lever-3-use-a-smaller-model.md)

# Lever 2: Stay in the cache window

*Reading time: about 9 minutes.*

> [!IMPORTANT]
> Pick the model, the effort level and the speed at the start of a task, and don't change them halfway through. Compact before a long break, or set a 1-hour cache time.

**Why it matters:** Each request sends the whole conversation again (see [lever
1](lever-1-keep-the-context-short.md)). Prompt caching lets the provider hold on
to its work on the start of the request, so it only has to process what's new
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching)). The start of
the request is the system prompt, the rules file and the earlier messages. The
model, the effort level and the speed count as part of it. The match has to be
exact: if one part changes, everything after it gets processed again.

The cache doesn't last. It expires after 5 minutes or 1 hour without a request.
Then the next request processes the whole conversation again. On the Anthropic
API, a cache read costs 0.1 times the normal input price. It costs 0.05 times on
Opus 5.5 and Sonnet 5.5, and 0.025 times on Fable 5.1 ([Anthropic
docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).
That's a price, not a measure of energy. But an expired cache means the provider
computes the whole conversation again, and that's what you're trying to avoid.
Other providers also cache the start of a prompt. OpenAI does it automatically
when the prompt has at least 1,024 tokens on its newest models ([OpenAI
docs](https://platform.openai.com/docs/guides/prompt-caching)).

<img alt="Two horizontal bars. In a normal turn, the system prompt, the rules file and the earlier messages are read from the cache, and only your new message and the new reply are processed. After a change at the start, for example a new model, the whole conversation is processed again." src="../images/cache-reuses-the-start.svg#gh-light-mode-only">
<img alt="Two horizontal bars. In a normal turn, the system prompt, the rules file and the earlier messages are read from the cache, and only your new message and the new reply are processed. After a change at the start, for example a new model, the whole conversation is processed again." src="../images/cache-reuses-the-start-dark.svg#gh-dark-mode-only">

| Item | Role |
|---|---|
| [2.1 Check how long your cache lasts](#21-check-how-long-your-cache-lasts) | Learn your cache time |
| [2.2 Compact before a break longer than your cache time](#22-compact-before-a-break-longer-than-your-cache-time) | Use the warm cache |
| [2.3 Set the cache time to 1 hour if you often take a break](#23-set-the-cache-time-to-1-hour-if-you-often-take-a-break) | Set your cache time, for every session or for one |
| [2.4 Do not change the model in the middle of a task](#24-do-not-change-the-model-in-the-middle-of-a-task) | Protect the cache: model |
| [2.5 Change the effort level, the speed and other settings only between tasks](#25-change-the-effort-level-the-speed-and-other-settings-only-between-tasks) | Protect the cache: everything else |

## 2.1 Check how long your cache lasts

**What:** Find out if your cache lasts 5 minutes or 1 hour.

**Why:** A break longer than your cache time ends the cache, and the next request
has to read the full conversation again. With a 1-hour cache time, the same break
costs nothing extra. The cache starts its time again with each request that uses
it ([Claude Code
docs](https://code.claude.com/docs/en/prompt-caching#cache-lifetime)).

<img alt="Two timelines that start at your last request. With a cache time of 5 minutes, the cache expires, and after a break of 10 minutes the next request processes the whole conversation again. With a cache time of 1 hour, the cache is still warm after 10 minutes, and the next request reads the conversation from the cache." src="../images/cache-time-5-minutes-or-1-hour.svg#gh-light-mode-only">
<img alt="Two timelines that start at your last request. With a cache time of 5 minutes, the cache expires, and after a break of 10 minutes the next request processes the whole conversation again. With a cache time of 1 hour, the cache is still warm after 10 minutes, and the next request reads the conversation from the cache." src="../images/cache-time-5-minutes-or-1-hour-dark.svg#gh-dark-mode-only">

**How:** In Claude Code, the cache time depends on how you pay. The table shows it
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#which-ttl-each-request-gets)):

| How you use Claude Code | Main conversation | Subagents and other requests |
|---|---|---|
| Claude subscription, inside the usage of your plan | 1 hour | 5 minutes |
| Usage credits, API key or cloud provider | 5 minutes | 5 minutes |

- The main conversation is your own messages and the work that Claude Code does
  with them.
- Other requests are the requests outside the main conversation. Examples are
  subagents, agent teams, compaction (see
  [1.5](lever-1-keep-the-context-short.md#15-compact-when-a-long-task-must-continue))
  and the titles of sessions.
- When you go over the limit of your plan and Claude Code uses usage credits,
  the main conversation drops to 5 minutes.
- The Anthropic API offers only two cache times: 5 minutes, which is the
  default, and 1 hour. Claude Code ignores any other value
  ([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).

**Where:**
- Terminal: `/usage` shows a **Prompt cache** line with the hit ratio and whether
  the cache is warm. It needs Claude Code 2.1.251 or later
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#check-cache-performance)).
- Desktop app, Code tab: the cache docs don't describe the desktop app. It runs
  Claude Code, so the table should apply there too.

## 2.2 Compact before a break longer than your cache time

**What:** Run `/compact` while the cache is warm, before a break that is longer
than the cache time.

**Why:** To make the summary, compacting sends the full conversation once. With a
warm cache, this request reads the conversation from the cache and "costs a
fraction of what the context size suggests". After a break longer than the cache
lifetime, it processes the full history again, with no cache
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#compacting-the-conversation)).

<img alt="Two timelines. When you run /compact while the cache is warm, the request reads the conversation from the cache, at a fraction of the cost of its size. When you run it after the cache expired, the request processes the full history again, with no cache." src="../images/compact-before-cache-expires.svg#gh-light-mode-only">
<img alt="Two timelines. When you run /compact while the cache is warm, the request reads the conversation from the cache, at a fraction of the cost of its size. When you run it after the cache expired, the request processes the full history again, with no cache." src="../images/compact-before-cache-expires-dark.svg#gh-dark-mode-only">

**How:** Compact at a natural break, before you step away. To learn your cache
time, see 2.1. For the effect of compaction on the context, see
[1.5](lever-1-keep-the-context-short.md#15-compact-when-a-long-task-must-continue).
To drop a failed attempt, `/rewind` is better than `/compact`: it goes back to a
part that is already in the cache (see
[1.4](lever-1-keep-the-context-short.md#14-go-back-with-rewind-when-an-attempt-failed)).

**Where:**
- Terminal and desktop app, Code tab: type `/compact` in the prompt box
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
- Terminal: on the 1-hour cache, Claude Code also compacts an idle conversation
  of 200K tokens or more. It does this about an hour after your last request,
  before the cache expires. The setting is `idleCompaction`, on by default
  ([Claude Code docs](https://code.claude.com/docs/en/settings-reference#idlecompaction)).

## 2.3 Set the cache time to 1 hour if you often take a break

**What:** Set `promptCacheTtl` to `1h`, or set the variable
`CLAUDE_CODE_PROMPT_CACHE_TTL=1h`. If you need 1 hour only for a long session with
many breaks, use the variable for that session only.

**Why:** With an API key, a cloud provider or usage credits, the main conversation
has a cache time of 5 minutes (see 2.1). A break of more than 5 minutes then
forces the next request to process the whole conversation again. A 1-hour cache
time avoids that.
- A cache write costs 2 times the normal input price with 1 hour, against 1.25
  times with 5 minutes. A cache read costs the same for both
  ([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).
- When the cache expires, the next request writes the whole conversation again.
  So 1 hour costs less if you often come back after 5 minutes to 1 hour.
- It costs more on short bursts of work with no stop of 5 minutes. The longer
  cache time is then not used
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#cache-lifetime)).
- The docs give prices, not energy. If you aren't sure and you often step away and
  come back, 1 hour is the safer choice, because it avoids computing the
  conversation again.

**How:** It needs Claude Code 2.1.242 or later. Each control takes `5m` or `1h`.
- To keep the value for every session, save it in a settings file.
- To use it for one session only, set the variable for that session. A value that
  you set for one session wins over the saved setting, so your settings file stays
  the same. To force 5 minutes, for example to compare the two, set
  `FORCE_PROMPT_CACHING_5M=1`.
- For subagents and other requests, the setting is `subagentPromptCacheTtl`
  (variable `CLAUDE_CODE_SUBAGENT_PROMPT_CACHE_TTL`).
- To check the result, run `claude -p "hello" --output-format json` and read
  `usage.cache_creation`
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#choose-the-ttl-yourself),
  [settings reference](https://code.claude.com/docs/en/settings-reference#promptcachettl)).

**Where:**
- Terminal, every session: add `"promptCacheTtl": "1h"` to a settings file, for
  example `~/.claude/settings.json`.
- Terminal, one session: start Claude Code with the variable, for example
  `CLAUDE_CODE_PROMPT_CACHE_TTL=1h claude`.
- Desktop app, Code tab, every local session: add the variable to the `env` key of
  `~/.claude/settings.json`, for example
  `"env": {"CLAUDE_CODE_PROMPT_CACHE_TTL": "1h"}`.
- Desktop app, Code tab, one session: add the variable to a saved local
  environment, then start the session from that environment. Only sessions that
  you start from it get the variable
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#set-environment-variables-for-local-sessions)).

**Watch out:** On a Claude subscription, inside the usage of your plan, the main
conversation already has 1 hour. You don't need this setting there.

## 2.4 Do not change the model in the middle of a task

**What:** Choose the model at the start of a task. Change it between tasks, for
example after `/clear`.

**Why:** Each model has its own cache. When you switch models, the next request
reads the whole conversation with no cache, even if the content is the same
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).

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
  don't describe the desktop app. Each model has its own cache, so a switch
  there can cost the same.

**Watch out:** Some actions switch the model without a clear sign
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).
- The `opusplan` model uses Opus in plan mode and Sonnet to run the plan. So each
  plan-mode toggle is a switch.
- An automatic fallback is a switch. A safety check can flag a request, and
  Claude Code then runs it again on a fallback model. The session continues
  there.
- A skill or command can set a `model` in its header. If it isn't your current
  model, that turn runs on it. Your model comes back at your next prompt.

## 2.5 Change the effort level, the speed and other settings only between tasks

**What:** Choose the effort level and the speed at the start of a task. Before you
change any other setting halfway through, check whether it resets the cache.

**Why:** The model isn't the only part of the start of the request. Each of these
changes can make the next request read the whole conversation again.
- **Effort level.** On most models, each level has its own cache. With an API key
  or a Claude subscription, the cache stays on Fable 5.1 and later, Opus 5.5,
  Sonnet 5.5 and Haiku 5.5. On Amazon Bedrock, Google Cloud's Agent Platform or a
  Claude apps gateway, a change of effort restarts the cache on every model
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#changing-effort-level)).
- **Speed.** Fast mode runs the same model on a setup that is up to 2.5 times
  faster, at a higher price for each token. Only some Opus and Fable models
  support it. On a subscription, it uses usage credits, not the usage of your
  plan ([Claude Code docs](https://code.claude.com/docs/en/fast-mode)). The two
  speeds keep separate caches. The first time you turn fast mode on in a
  conversation, the next request reads the whole conversation again, at fast mode
  rates. Each later toggle also reads uncached
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#turning-on-fast-mode)).
  The docs give the speed and the price, not the energy of fast mode.
- **Other changes.** The docs list more actions that can invalidate the cache
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#actions-that-invalidate-the-cache)).
  They are connecting or removing an MCP server, enabling or disabling a plugin,
  and denying a whole tool. Compacting, many images in the conversation and an
  upgrade of Claude Code also count. Some of them cost nothing in some cases. For example,
  tool search is on by default on supported models. Then a server that connects
  in the middle of a session doesn't change what is cached.

**How:**
- Choose the effort level at the start (see [lever 4](lever-4-lower-the-effort.md)).
  On a model that doesn't keep the cache, Claude Code asks you to confirm a
  change while the cache is warm.
- Turn on fast mode before the first message of the session, or don't use it.
  Run `/fast` again to see if it is on. A small `↯` icon shows next to the prompt
  while it is on
  ([Claude Code docs](https://code.claude.com/docs/en/fast-mode#toggle-fast-mode)).
- Make the other changes between tasks, or before the first message. Read the
  section of the docs for the change that you plan.

**Where:**
- Terminal: `/effort` for the effort level, and `/fast` or `"fastMode": true` in
  your user settings file for fast mode. `/usage` shows the cache hit ratio (see
  2.1).
- VS Code extension: the **Toggle fast mode** command.
- Desktop app, Code tab: Cmd+Shift+E opens the effort menu
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)). The cache docs
  don't describe the desktop app. The fast mode docs name only the terminal and
  the VS Code extension. If `/fast` appears in the `/` menu of the prompt box, the
  same advice applies.

**Watch out:**
- If your model doesn't support fast mode, `/fast` also switches you to Opus.
  This is a model switch (see 2.4).
- If you hit a rate limit, Claude Code goes back to standard speed by itself.
  Later it goes back to fast mode. Each of these changes of speed loses the cache
  of the other speed
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#turning-on-fast-mode)).
- Some changes keep the cache: editing files, changing the permission mode, using
  a skill, and spawning a subagent. Editing `CLAUDE.md` also keeps the cache, but
  the change applies only after `/clear`, `/compact` or a restart
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#actions-that-keep-the-cache)).

---

[← Lever 1: Keep the context short](lever-1-keep-the-context-short.md) &emsp; [All levers](../README.md) &emsp; [Lever 3: Use a smaller model →](lever-3-use-a-smaller-model.md)
