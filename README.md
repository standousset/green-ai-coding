# Green AI coding

Code with AI and use less energy. 6 levers, the largest first, each with its
sources.

## Copy this (1 minute)

**For the assistant.** Paste this into your `CLAUDE.md`, `AGENTS.md`, rules
file or prompt:

```text
Help me use less energy and fewer tokens:
- Be brief. Show only the code that changes. No summary at the end unless I ask.
- Read only the files that the task needs. Ask before you read the whole project.
- Summarise long outputs (tests, logs, searches): give the result, not the full text.
- Make the smallest change that works. No extra features, files or dependencies.
- If a standard tool can do the task without AI (search, rename, formatter, linter), tell me.
- If my request is not clear, ask one question before you start.
```

**For Claude Code.** Paste this into `~/.claude/settings.json` (for you) or
`.claude/settings.json` (for the whole project):

```json
{
  "model": "sonnet",
  "effortLevel": "medium"
}
```

Then use `/effort high` or `/model opus` only for hard tasks.

## The 6 levers

| | Lever | Why it matters |
|---|---|---|
| 1 | [**Lower the effort**](levers/1-lower-the-effort.md) | The same model can use **150 to 700 times** more energy with reasoning on than off. |
| 2 | [**Use a smaller model**](levers/2-use-a-smaller-model.md) | The energy for each token grows with the size of the model. |
| 3 | [**Keep the context short**](levers/3-keep-the-context-short.md) | The full conversation is sent again with **every** request. |
| 4 | [**Stay in the cache window**](levers/4-stay-in-the-cache-window.md) | A cache read costs **0.1 times** the price or less, but each model has its own cache. |
| 5 | [**Cut tools and side work**](levers/5-cut-tools-and-side-work.md) | Tool servers, tests and logs fill the context. A small subagent keeps them out. |
| 6 | [**Make fewer round trips**](levers/6-make-fewer-round-trips.md) | Each retry sends the full context again. |
| ★ | [**Bonus: a low-carbon region**](levers/bonus-low-carbon-region.md) | Electricity emits from **28 gCO₂/kWh** (Norway) to **670** (India). |

Each page tells you why the lever matters, how to apply it, what to copy, and
how to check the result.

## Where the energy goes

A request to an AI assistant uses more energy when the model:
- **writes** more tokens (thinking tokens included): levers 1 and 6;
- is **larger**: lever 2;
- **reads** more tokens that are not in the cache: levers 3, 4 and 5.

The electricity of the data centre then sets the CO₂: bonus lever.

## Licence

[MIT](LICENSE)
