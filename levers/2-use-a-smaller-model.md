[← 1 · Lower the effort](1-lower-the-effort.md) · [All levers](../README.md) · [3 · Keep the context short →](3-keep-the-context-short.md)

# 2 · Use a smaller model

**In one line:** make a mid-size model your default, and use the largest model
for the hardest tasks only.

## Why it matters

- The energy to generate one token grows with the number of active parameters
  of the model ([EcoLogits method](https://ecologits.ai/latest/methodology/llm_inference/)).
  A larger model uses more energy for each word that it writes.
- The Claude Code documentation says: "Sonnet handles most coding tasks well".
  It keeps Opus for complex architecture and multi-step reasoning
  ([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model)).

## How to apply

**Claude Code**
- Set a default: `"model": "sonnet"` in your settings (see below).
- Change the model for one task with `/model`, at the start of the task and
  after `/clear`. Each model has its own cache, so a change in the middle of a
  task reads the full conversation again
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).
- `/model` also saves your choice as the default for new sessions, in your user
  settings
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#setting-your-model)).
  To switch for this session only, open `/model`, choose the model and press
  `s`.
- If the project or local settings file also has a `model` key, that key wins
  over your user settings. The next session then starts on the project model,
  whatever you saved with `/model`.
- Option: `opusplan` uses Opus in plan mode and Sonnet for the work
  ([model aliases](https://code.claude.com/docs/en/model-config#model-aliases)).
  Each switch between plan mode and work mode starts a new cache. It fits one
  plan followed by a long work phase, and it costs more if you switch often.

**Other assistants:** use the model picker. Choose a mid-size model as the
default. If the assistant has an automatic mode, check which models it uses.

**Teams:** set the default model in the project settings file. Make the largest
model an option for hard tasks.

## Copy this

Add this key to `.claude/settings.json` in your project (for everyone on the
project), to `.claude/settings.local.json` (only for you, in this project), or
to `~/.claude/settings.json` (for you, in all projects):

```json
{
  "model": "sonnet"
}
```

## Check it

- Try the smaller model first on a type of task. Go to a larger model only when
  the result is not good enough.
- `/usage` in Claude Code shows the tokens of the session, for each model.

## Resources

- [AI Energy Score leaderboard](https://huggingface.co/spaces/AIEnergyScore/Leaderboard)
  and [ML.ENERGY leaderboard](https://ml.energy/leaderboard): measured energy of
  open models, by task and size.
- [EcoLogits](https://ecologits.ai/): estimates the energy and emissions of
  requests to AI providers, and explains the method.
- [Model configuration](https://code.claude.com/docs/en/model-config) (Claude
  Code docs).

---

[← 1 · Lower the effort](1-lower-the-effort.md) · [All levers](../README.md) · [3 · Keep the context short →](3-keep-the-context-short.md)
