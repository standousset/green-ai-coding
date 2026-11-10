# Green AI coding

A short guide to reduce the energy and carbon impact that AI coding assistants use.

Each token that an assistant reads or writes uses electricity in a data centre.

## Copy this: permanent rules

Paste this into the rules file of your assistant (`AGENTS.md`, `CLAUDE.md` or similar). The assistant then follows it in every session:

```text
Help me use less energy and fewer tokens:
- Be brief. Show only the code that changes. No summary at the end unless I ask.
- Read only the files that the task needs. Ask before you read the whole project.
- Summarise long outputs (tests, logs, searches): give the result, not the full text.
- Make the smallest change that works. No extra features, files or dependencies.
- Write code that is light to run: avoid needless loops, queries, network calls and large data.
- If a standard tool can do the task without AI (search, rename, formatter, linter), tell me.
- If my request is not clear, ask all the questions needed before starting.
```

## Copy this: audit your setup

Paste this into a prompt. The assistant checks your settings and your habits against the levers, and tells you what to change:

```text
Audit how I use you, to reduce energy and tokens. Use the 6 levers of https://github.com/standousset/green-ai-coding as a checklist. If you cannot open the link, use your own knowledge and say so.
1. Context: how long are my rules or custom instructions (for example CLAUDE.md, AGENTS.md, or a settings field)? Do my sessions get very long? When does compaction start?
2. Cache: which model and effort level do I use? Do I change them in the middle of a task? How long does the cache last for me?
3. Model: is my default model larger than my tasks need?
4. Effort: is my default effort level higher than my tasks need?
5. Round trips: which of my recent prompts were vague or led to retries? Which tasks could a standard tool do?
6. Region: can I choose where my requests run?
Find the settings and rules files of this tool yourself, for me and for this project, and read only those files. Do not read the whole project and do not change anything. If you cannot read a setting (for example in a desktop app or a chat window), tell me where to find it, and ask me for its value. Be direct. Then give me at most 5 changes, in order of impact: what you found, the change to make, and the reason. Say what you cannot check.
```

## The 6 levers

| | Lever | Why it matters |
|---|---|---|
| 1 | [Keep the context short](levers/1-keep-the-context-short.md) | The full conversation is sent again with every request. Noisy work (tests, logs, searches) fills it fast. A subagent (a helper agent with its own context) keeps that work out, and compaction (a summary that replaces the old conversation) shrinks what remains. A long context also makes the answers less reliable. |
| 2 | [Stay in the cache window](levers/2-stay-in-the-cache-window.md) | A cache read costs 0.1 times the normal input price or less. The cache lasts 5 minutes or 1 hour, depending on how you pay, and each model has its own cache. A change at the start of the conversation, such as a new model, makes the cache useless. Compact while the cache is warm. |
| 3 | [Use a smaller model](levers/3-use-a-smaller-model.md) | The energy for each token grows with the size of the model. Many plans start on the largest model. Make Sonnet your default, and use Opus for the hardest tasks only. |
| 4 | [Lower the effort](levers/4-lower-the-effort.md) | Thinking makes the model write more tokens. In a test of three open models, reasoning on used 150 to 700 times more energy than off. Use a low or medium level for most tasks. |
| 5 | [Make fewer round trips](levers/5-make-fewer-round-trips.md) | Each retry sends the full context again. |
| 6 | [Choose a low-carbon region](levers/6-choose-a-low-carbon-region.md) | Electricity production emits between 28 gCO₂/kWh (Norway) and 670 (India), more than 20 times more. This applies only if you can choose where requests run. |

## Contents

- [1 Keep the context short](levers/1-keep-the-context-short.md)
  - [1.1 Drop old context that the task does not need](levers/1-keep-the-context-short.md#11-drop-old-context-that-the-task-does-not-need)
  - [1.2 Send only what the task needs](levers/1-keep-the-context-short.md#12-send-only-what-the-task-needs)
  - [1.3 Give noisy work to a subagent](levers/1-keep-the-context-short.md#13-give-noisy-work-to-a-subagent)
  - [1.4 Go back with `/rewind` when a path did not work](levers/1-keep-the-context-short.md#14-go-back-with-rewind-when-a-path-did-not-work)
  - [1.5 Compact when a long task must continue](levers/1-keep-the-context-short.md#15-compact-when-a-long-task-must-continue)
  - [1.6 Set when auto-compaction starts](levers/1-keep-the-context-short.md#16-set-when-auto-compaction-starts)
- [2 Stay in the cache window](levers/2-stay-in-the-cache-window.md)
  - [2.1 Keep the start of the conversation unchanged](levers/2-stay-in-the-cache-window.md#21-keep-the-start-of-the-conversation-unchanged)
  - [2.2 Check your cache time before you take a break](levers/2-stay-in-the-cache-window.md#22-check-your-cache-time-before-you-take-a-break)
  - [2.3 Set the cache time to 1 hour if you often come back after a break](levers/2-stay-in-the-cache-window.md#23-set-the-cache-time-to-1-hour-if-you-often-come-back-after-a-break)
  - [2.4 Do not change the model in the middle of a task](levers/2-stay-in-the-cache-window.md#24-do-not-change-the-model-in-the-middle-of-a-task)
  - [2.5 Change the effort level in the middle of a task only on a model that keeps the cache](levers/2-stay-in-the-cache-window.md#25-change-the-effort-level-in-the-middle-of-a-task-only-on-a-model-that-keeps-the-cache)
  - [2.6 Turn on fast mode at the start of a session](levers/2-stay-in-the-cache-window.md#26-turn-on-fast-mode-at-the-start-of-a-session)
  - [2.7 Compact while the cache is warm](levers/2-stay-in-the-cache-window.md#27-compact-while-the-cache-is-warm)
- [3 Use a smaller model](levers/3-use-a-smaller-model.md)
  - [3.1 Know that a larger model uses more energy for each token](levers/3-use-a-smaller-model.md#31-know-that-a-larger-model-uses-more-energy-for-each-token)
  - [3.2 Check which model you use now](levers/3-use-a-smaller-model.md#32-check-which-model-you-use-now)
  - [3.3 Make Sonnet your default model](levers/3-use-a-smaller-model.md#33-make-sonnet-your-default-model)
  - [3.4 Try the smaller model first and go up only when needed](levers/3-use-a-smaller-model.md#34-try-the-smaller-model-first-and-go-up-only-when-needed)
  - [3.5 Press `s` in the `/model` picker to switch for one session only](levers/3-use-a-smaller-model.md#35-press-s-in-the-model-picker-to-switch-for-one-session-only)
  - [3.6 Use `opusplan` for one plan followed by a long work phase](levers/3-use-a-smaller-model.md#36-use-opusplan-for-one-plan-followed-by-a-long-work-phase)
  - [3.7 Give simple subagent tasks to Haiku](levers/3-use-a-smaller-model.md#37-give-simple-subagent-tasks-to-haiku)
- [4 Lower the effort](levers/4-lower-the-effort.md)
  - [4.1 Know that more thinking means more tokens and more energy](levers/4-lower-the-effort.md#41-know-that-more-thinking-means-more-tokens-and-more-energy)
  - [4.2 Check your effort level now](levers/4-lower-the-effort.md#42-check-your-effort-level-now)
  - [4.3 Use a low or medium level for most coding tasks and go up only when needed](levers/4-lower-the-effort.md#43-use-a-low-or-medium-level-for-most-coding-tasks-and-go-up-only-when-needed)
  - [4.4 Use `ultrathink` for one hard turn instead of raising the level](levers/4-lower-the-effort.md#44-use-ultrathink-for-one-hard-turn-instead-of-raising-the-level)
  - [4.5 Press `s` to change the level for this session only](levers/4-lower-the-effort.md#45-press-s-to-change-the-level-for-this-session-only)
  - [4.6 Set a default level for a project](levers/4-lower-the-effort.md#46-set-a-default-level-for-a-project)
  - [4.7 Give simple subagent tasks a low effort](levers/4-lower-the-effort.md#47-give-simple-subagent-tasks-a-low-effort)
- [5 Make fewer round trips](levers/5-make-fewer-round-trips.md)
  - [5.1 A retry costs more than the first attempt](levers/5-make-fewer-round-trips.md#51-a-retry-costs-more-than-the-first-attempt)
  - [5.2 Ask precisely](levers/5-make-fewer-round-trips.md#52-ask-precisely)
  - [5.3 Plan large tasks first](levers/5-make-fewer-round-trips.md#53-plan-large-tasks-first)
  - [5.4 Do not use AI when a simple tool does the job](levers/5-make-fewer-round-trips.md#54-do-not-use-ai-when-a-simple-tool-does-the-job)
- [6 Choose a low-carbon region](levers/6-choose-a-low-carbon-region.md)
  - [6.1 The same energy emits very different amounts of CO₂](levers/6-choose-a-low-carbon-region.md#61-the-same-energy-emits-very-different-amounts-of-co₂)
  - [6.2 Choose the region when you can](levers/6-choose-a-low-carbon-region.md#62-choose-the-region-when-you-can)
  - [6.3 Find the carbon data of a region](levers/6-choose-a-low-carbon-region.md#63-find-the-carbon-data-of-a-region)

## Licence

[MIT](LICENSE)
