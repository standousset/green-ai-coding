# Green AI coding

A short guide to reduce the energy that AI coding assistants use.

Each token that an assistant reads or writes uses electricity in a data centre. This guide gives 7 levers (settings and habits) that cut the number of tokens, the energy for each token, or the carbon of that energy. Each lever has a reason, steps to follow, and text or settings that you can copy. The exact commands are for Claude Code. The ideas apply to any AI coding assistant.

## Copy this

Paste this into the rules file of your assistant (`AGENTS.md`, `CLAUDE.md` or similar), or into a prompt:

```text
Help me use less energy and fewer tokens:
- Be brief. Show only the code that changes. No summary at the end unless I ask.
- Read only the files that the task needs. Ask before you read the whole project.
- Summarise long outputs (tests, logs, searches): give the result, not the full text.
- Make the smallest change that works. No extra features, files or dependencies.
- Write code that is light to run: avoid needless loops, queries, network calls and large data.
- If a standard tool can do the task without AI (search, rename, formatter, linter), tell me.
- If my request is not clear, ask all your questions in one message before you start.
```

## The 7 levers

| | Lever | Why it matters |
|---|---|---|
| 1 | [Lower the effort](levers/1-lower-the-effort.md) | The same model can use 150 to 700 times more energy with reasoning on than off. |
| 2 | [Use a smaller model](levers/2-use-a-smaller-model.md) | The energy for each token grows with the size of the model. |
| 3 | [Keep the context short](levers/3-keep-the-context-short.md) | The full conversation is sent again with every request. |
| 4 | [Stay in the cache window](levers/4-stay-in-the-cache-window.md) | A cache read costs 0.1 times the normal input price or less. The cache lasts 5 minutes or 1 hour, depending on how you pay, and each model has its own cache. |
| 5 | [Cut tools and side work](levers/5-cut-tools-and-side-work.md) | Tool servers, tests and logs fill the context. A subagent (a helper agent with its own context) on a small model keeps them out. |
| 6 | [Make fewer round trips](levers/6-make-fewer-round-trips.md) | Each retry sends the full context again. |
| 7 | [Choose a low-carbon region](levers/7-choose-a-low-carbon-region.md) | Electricity production emits between 28 gCO₂/kWh (Norway) and 670 (India), more than 20 times more. This applies only if you can choose where requests run. |

## Licence

[MIT](LICENSE)
