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
1. Context: how long are my rules or custom instructions (for example CLAUDE.md, AGENTS.md, or a settings field)? Which tool servers (MCP), plugins or extensions are connected? Do my sessions get very long? When does compaction start?
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
| 1 | [Keep the context short](levers/1-keep-the-context-short.md) | The full conversation is sent again with every request. Noisy work (tests, logs, searches) fills it fast. A subagent (a helper agent with its own context) keeps that work out, and compaction (a summary that replaces the old conversation) shrinks what remains. Compact while the cache is warm. |
| 2 | [Stay in the cache window](levers/2-stay-in-the-cache-window.md) | A cache read costs 0.1 times the normal input price or less. The cache lasts 5 minutes or 1 hour, depending on how you pay, and each model has its own cache. |
| 3 | [Use a smaller model](levers/3-use-a-smaller-model.md) | The energy for each token grows with the size of the model. |
| 4 | [Lower the effort](levers/4-lower-the-effort.md) | The same model can use 150 to 700 times more energy with reasoning on than off. |
| 5 | [Make fewer round trips](levers/5-make-fewer-round-trips.md) | Each retry sends the full context again. |
| 6 | [Choose a low-carbon region](levers/6-choose-a-low-carbon-region.md) | Electricity production emits between 28 gCO₂/kWh (Norway) and 670 (India), more than 20 times more. This applies only if you can choose where requests run. |

## Contents

- [1 · Keep the context short](levers/1-keep-the-context-short.md)
  - [The assistant sends the whole conversation with every request](levers/1-keep-the-context-short.md#the-assistant-sends-the-whole-conversation-with-every-request)
  - [A long context makes the answers less reliable](levers/1-keep-the-context-short.md#a-long-context-makes-the-answers-less-reliable)
  - [Compact while the cache is warm](levers/1-keep-the-context-short.md#compact-while-the-cache-is-warm)
  - [Choose when auto-compaction happens](levers/1-keep-the-context-short.md#choose-when-auto-compaction-happens)
  - [Idle compaction](levers/1-keep-the-context-short.md#idle-compaction)
  - [Go back with `/rewind` when a path did not work](levers/1-keep-the-context-short.md#go-back-with-rewind-when-a-path-did-not-work)
  - [Give noisy work to a subagent](levers/1-keep-the-context-short.md#give-noisy-work-to-a-subagent)
  - [Send only what the task needs](levers/1-keep-the-context-short.md#send-only-what-the-task-needs)
  - [Disconnect the tools that you do not use](levers/1-keep-the-context-short.md#disconnect-the-tools-that-you-do-not-use)
- [2 · Stay in the cache window](levers/2-stay-in-the-cache-window.md)
  - [Prompt caching reuses the start of the conversation](levers/2-stay-in-the-cache-window.md#prompt-caching-reuses-the-start-of-the-conversation)
  - [The cache lasts 5 minutes or 1 hour](levers/2-stay-in-the-cache-window.md#the-cache-lasts-5-minutes-or-1-hour)
  - [Set the cache time yourself](levers/2-stay-in-the-cache-window.md#set-the-cache-time-yourself)
  - [Do not change the model in the middle of a task](levers/2-stay-in-the-cache-window.md#do-not-change-the-model-in-the-middle-of-a-task)
  - [A change of effort may keep the cache](levers/2-stay-in-the-cache-window.md#a-change-of-effort-may-keep-the-cache)
  - [Compact before you step away](levers/2-stay-in-the-cache-window.md#compact-before-you-step-away)
  - [Start a new session after a long break](levers/2-stay-in-the-cache-window.md#start-a-new-session-after-a-long-break)
  - [Keep the start of the prompt the same in your own scripts](levers/2-stay-in-the-cache-window.md#keep-the-start-of-the-prompt-the-same-in-your-own-scripts)
- [3 · Use a smaller model](levers/3-use-a-smaller-model.md)
  - [A larger model uses more energy for each token](levers/3-use-a-smaller-model.md#a-larger-model-uses-more-energy-for-each-token)
  - [Make a mid-size model your default](levers/3-use-a-smaller-model.md#make-a-mid-size-model-your-default)
  - [Change the model with `/model` at the start of a task](levers/3-use-a-smaller-model.md#change-the-model-with-model-at-the-start-of-a-task)
  - [`/model` also changes your default](levers/3-use-a-smaller-model.md#model-also-changes-your-default)
  - [Use `opusplan` for one plan and a long work phase](levers/3-use-a-smaller-model.md#use-opusplan-for-one-plan-and-a-long-work-phase)
  - [Give simple subagents a small model](levers/3-use-a-smaller-model.md#give-simple-subagents-a-small-model)
- [4 · Lower the effort](levers/4-lower-the-effort.md)
  - [Thinking uses most of the energy](levers/4-lower-the-effort.md#thinking-uses-most-of-the-energy)
  - [On the newest Claude models, effort is your only control](levers/4-lower-the-effort.md#on-the-newest-claude-models-effort-is-your-only-control)
  - [Use a low level for most coding tasks](levers/4-lower-the-effort.md#use-a-low-level-for-most-coding-tasks)
  - [Check the default level](levers/4-lower-the-effort.md#check-the-default-level)
  - [Change the level for one task with `/effort`](levers/4-lower-the-effort.md#change-the-level-for-one-task-with-effort)
  - [Set a default level for a project](levers/4-lower-the-effort.md#set-a-default-level-for-a-project)
  - [Choose the level at the start of a task](levers/4-lower-the-effort.md#choose-the-level-at-the-start-of-a-task)
- [5 · Make fewer round trips](levers/5-make-fewer-round-trips.md)
  - [A retry costs more than the first attempt](levers/5-make-fewer-round-trips.md#a-retry-costs-more-than-the-first-attempt)
  - [Ask precisely](levers/5-make-fewer-round-trips.md#ask-precisely)
  - [Plan large tasks first](levers/5-make-fewer-round-trips.md#plan-large-tasks-first)
  - [Do not use AI when a simple tool does the job](levers/5-make-fewer-round-trips.md#do-not-use-ai-when-a-simple-tool-does-the-job)
- [6 · Choose a low-carbon region](levers/6-choose-a-low-carbon-region.md)
  - [The same energy emits very different amounts of CO₂](levers/6-choose-a-low-carbon-region.md#the-same-energy-emits-very-different-amounts-of-co₂)
  - [Choose the region when you can](levers/6-choose-a-low-carbon-region.md#choose-the-region-when-you-can)
  - [Find the carbon data of a region](levers/6-choose-a-low-carbon-region.md#find-the-carbon-data-of-a-region)

## Licence

[MIT](LICENSE)
