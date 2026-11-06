# Green AI coding

A short guide to reduce the energy and carbon impact that AI coding assistants use.

Each token that an assistant reads or writes uses electricity in a data centre.

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
- If my request is not clear, ask all the questions needed before starting.
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

## Licence

[MIT](LICENSE)
