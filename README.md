# Green AI coding

A short guide to reduce the energy and carbon impact that AI coding assistants use.

## A - The 6 levers

### [Lever 1: Keep the context short](levers/lever-1-keep-the-context-short.md)
The full conversation is sent again with every request. Noisy work (tests, logs, searches) fills it fast. A subagent (a helper agent with its own context) keeps that work out, and compaction (a summary that replaces the old conversation) shrinks what remains. A long context also makes the answers less reliable.
- [1.1 Drop old context that the task does not need](levers/lever-1-keep-the-context-short.md#11-drop-old-context-that-the-task-does-not-need)
- [1.2 Send only what the task needs](levers/lever-1-keep-the-context-short.md#12-send-only-what-the-task-needs)
- [1.3 Give noisy work to a subagent](levers/lever-1-keep-the-context-short.md#13-give-noisy-work-to-a-subagent)
- [1.4 Go back with `/rewind` when a path did not work](levers/lever-1-keep-the-context-short.md#14-go-back-with-rewind-when-a-path-did-not-work)
- [1.5 Compact when a long task must continue](levers/lever-1-keep-the-context-short.md#15-compact-when-a-long-task-must-continue)
- [1.6 Set when auto-compaction starts](levers/lever-1-keep-the-context-short.md#16-set-when-auto-compaction-starts)

### [Lever 2: Stay in the cache window](levers/lever-2-stay-in-the-cache-window.md)
A cache read costs 0.1 times the normal input price or less. The cache lasts 5 minutes or 1 hour, depending on how you pay, and each model has its own cache. A change at the start of the conversation, such as a new model, makes the cache useless. Compact while the cache is warm.
- [2.1 Keep the start of the conversation unchanged](levers/lever-2-stay-in-the-cache-window.md#21-keep-the-start-of-the-conversation-unchanged)
- [2.2 Check your cache time before you take a break](levers/lever-2-stay-in-the-cache-window.md#22-check-your-cache-time-before-you-take-a-break)
- [2.3 Set the cache time to 1 hour if you often come back after a break](levers/lever-2-stay-in-the-cache-window.md#23-set-the-cache-time-to-1-hour-if-you-often-come-back-after-a-break)
- [2.4 Do not change the model in the middle of a task](levers/lever-2-stay-in-the-cache-window.md#24-do-not-change-the-model-in-the-middle-of-a-task)
- [2.5 Change the effort level in the middle of a task only on a model that keeps the cache](levers/lever-2-stay-in-the-cache-window.md#25-change-the-effort-level-in-the-middle-of-a-task-only-on-a-model-that-keeps-the-cache)
- [2.6 Turn on fast mode at the start of a session](levers/lever-2-stay-in-the-cache-window.md#26-turn-on-fast-mode-at-the-start-of-a-session)
- [2.7 Compact while the cache is warm](levers/lever-2-stay-in-the-cache-window.md#27-compact-while-the-cache-is-warm)

### [Lever 3: Use a smaller model](levers/lever-3-use-a-smaller-model.md)
The energy for each token grows with the size of the model. Many plans start on the largest model. Make Sonnet your default, and use Opus for the hardest tasks only.
- [3.1 Know that a larger model uses more energy for each token](levers/lever-3-use-a-smaller-model.md#31-know-that-a-larger-model-uses-more-energy-for-each-token)
- [3.2 Check which model you use now](levers/lever-3-use-a-smaller-model.md#32-check-which-model-you-use-now)
- [3.3 Make Sonnet your default model](levers/lever-3-use-a-smaller-model.md#33-make-sonnet-your-default-model)
- [3.4 Try the smaller model first and go up only when needed](levers/lever-3-use-a-smaller-model.md#34-try-the-smaller-model-first-and-go-up-only-when-needed)
- [3.5 Press `s` in the `/model` picker to switch for one session only](levers/lever-3-use-a-smaller-model.md#35-press-s-in-the-model-picker-to-switch-for-one-session-only)
- [3.6 Use `opusplan` for one plan followed by a long work phase](levers/lever-3-use-a-smaller-model.md#36-use-opusplan-for-one-plan-followed-by-a-long-work-phase)
- [3.7 Give simple subagent tasks to Haiku](levers/lever-3-use-a-smaller-model.md#37-give-simple-subagent-tasks-to-haiku)

### [Lever 4: Lower the effort](levers/lever-4-lower-the-effort.md)
Thinking makes the model write more tokens. In a test of three open models, reasoning on used 150 to 700 times more energy than off. Use a low or medium level for most tasks.
- [4.1 Know that more thinking means more tokens and more energy](levers/lever-4-lower-the-effort.md#41-know-that-more-thinking-means-more-tokens-and-more-energy)
- [4.2 Check your effort level now](levers/lever-4-lower-the-effort.md#42-check-your-effort-level-now)
- [4.3 Use a low or medium level for most coding tasks and go up only when needed](levers/lever-4-lower-the-effort.md#43-use-a-low-or-medium-level-for-most-coding-tasks-and-go-up-only-when-needed)
- [4.4 Use `ultrathink` for one hard turn instead of raising the level](levers/lever-4-lower-the-effort.md#44-use-ultrathink-for-one-hard-turn-instead-of-raising-the-level)
- [4.5 Press `s` to change the level for this session only](levers/lever-4-lower-the-effort.md#45-press-s-to-change-the-level-for-this-session-only)
- [4.6 Set a default level for a project](levers/lever-4-lower-the-effort.md#46-set-a-default-level-for-a-project)
- [4.7 Give simple subagent tasks a low effort](levers/lever-4-lower-the-effort.md#47-give-simple-subagent-tasks-a-low-effort)

### [Lever 5: Make fewer round trips](levers/lever-5-make-fewer-round-trips.md)
Each retry sends the whole conversation again, and the failed reply is part of it. Name the file, plan a large task first, and run `/clear` after two failed corrections.
- [5.1 Know that a retry costs more than the first attempt](levers/lever-5-make-fewer-round-trips.md#51-know-that-a-retry-costs-more-than-the-first-attempt)
- [5.2 Name the file, the limit and the result in your first request](levers/lever-5-make-fewer-round-trips.md#52-name-the-file-the-limit-and-the-result-in-your-first-request)
- [5.3 Ask Claude to interview you when the task is not clear](levers/lever-5-make-fewer-round-trips.md#53-ask-claude-to-interview-you-when-the-task-is-not-clear)
- [5.4 Plan a large task first](levers/lever-5-make-fewer-round-trips.md#54-plan-a-large-task-first)
- [5.5 Give Claude a check to run](levers/lever-5-make-fewer-round-trips.md#55-give-claude-a-check-to-run)
- [5.6 Press Esc as soon as Claude goes the wrong way](levers/lever-5-make-fewer-round-trips.md#56-press-esc-as-soon-as-claude-goes-the-wrong-way)
- [5.7 After two failed corrections, run `/clear` and write a better prompt](levers/lever-5-make-fewer-round-trips.md#57-after-two-failed-corrections-run-clear-and-write-a-better-prompt)
- [5.8 Use a simple tool when it does the job](levers/lever-5-make-fewer-round-trips.md#58-use-a-simple-tool-when-it-does-the-job)

### [Lever 6: Choose a low-carbon region](levers/lever-6-choose-a-low-carbon-region.md)
In 2025, electricity emitted 28 gCO₂/kWh in Norway and 670 in India, more than 20 times more. This applies only if you can choose where requests run, for example on Amazon Bedrock or Google Cloud.
- [6.1 Know that the same energy emits very different amounts of CO₂](levers/lever-6-choose-a-low-carbon-region.md#61-know-that-the-same-energy-emits-very-different-amounts-of-co₂)
- [6.2 Check whether you can choose a region](levers/lever-6-choose-a-low-carbon-region.md#62-check-whether-you-can-choose-a-region)
- [6.3 Find the carbon data of a region](levers/lever-6-choose-a-low-carbon-region.md#63-find-the-carbon-data-of-a-region)
- [6.4 Choose a region with low-carbon electricity when you can](levers/lever-6-choose-a-low-carbon-region.md#64-choose-a-region-with-low-carbon-electricity-when-you-can)
- [6.5 Check where your requests really run](levers/lever-6-choose-a-low-carbon-region.md#65-check-where-your-requests-really-run)


## B - Copy this: permanent rules

Paste this into the rules file of your assistant. The assistant then follows it in every session.

Claude Code reads `AGENTS.md` only when no `CLAUDE.md` exists. If you have a `CLAUDE.md`, write `@AGENTS.md` in it ([Claude Code docs](https://code.claude.com/docs/en/memory#agents-md)).

A rules file acts on levers 1 and 5. The other levers are settings: use the audit below for them.

```text
Help me use less energy and fewer tokens:
- Be brief. Show only the code that changes. No summary at the end unless I ask.
- Read only the files that the task needs. Ask before you read the whole project.
- Keep long outputs (tests, logs, searches) small: filter them at the source, and show me only the result.
- Make the smallest change that works. No extra features, files or dependencies.
- Write code that is light to run: avoid needless loops, queries, network calls and large data.
- If a standard tool can do the task without AI (search, rename, formatter, linter), tell me.
- If my request is not clear, ask all the questions needed before starting.
```

## C - Copy this: audit your setup

Paste this into a prompt. The assistant checks your settings and your habits against the levers, and tells you what to change:

```text
Audit how I use you, to reduce energy and tokens. Use the 6 levers below as a checklist. For details, read the README of https://github.com/standousset/green-ai-coding and open a lever page only when you need it. If you cannot open the link, use your own knowledge and say so.
1. Context: how long are my rules or custom instructions (for example CLAUDE.md, AGENTS.md, or a settings field)? Is a rules file longer than 200 lines? Could parts of it move to skills or path-scoped rules? Do my sessions get very long? When does compaction start?
2. Cache: how do I pay (subscription or API), and so how long does the cache last? Do I change the model or the effort in the middle of a task? Do I take long breaks?
3. Model: is my default model larger than my tasks need?
4. Effort: is my default effort level higher than my tasks need?
5. Round trips: in this conversation, which of my prompts were vague or led to retries? Which tasks could a standard tool do?
6. Region: can I choose where my requests run?
Find the settings and rules files of the tool that runs you yourself, at user level and at project level. Read only those files, this conversation, and the list of skills, plugins and connectors that you load. Do not read the whole project and do not change anything. If you cannot read a setting (for example in a desktop app or a chat window), tell me where to find it, and ask me for its value. Be direct. Then give me at most 5 changes, in order of impact: what you found, the change to make, and the reason. Say what you cannot check. End with the list of values you need from me.
```


## License

[MIT](LICENSE)
