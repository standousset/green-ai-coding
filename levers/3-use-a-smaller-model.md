[← 2 · Stay in the cache window](2-stay-in-the-cache-window.md) · [All levers](../README.md) · [4 · Lower the effort →](4-lower-the-effort.md)

# 3 · Use a smaller model

**In one line:** make a mid-size model your default, and use the largest model
for the hardest tasks only.

## A larger model uses more energy for each token

The energy to generate one token grows with the number of active parameters of
the model ([EcoLogits method](https://ecologits.ai/latest/methodology/llm_inference/)).
A larger model uses more energy for each word that it writes. The
[AI Energy Score leaderboard](https://huggingface.co/spaces/AIEnergyScore/Leaderboard)
and the [ML.ENERGY leaderboard](https://ml.energy/leaderboard) show measured
energy of open models, by task and size.

## Make a mid-size model your default

The Claude Code documentation says: "Sonnet handles most coding tasks well". It
keeps Opus for complex architecture and multi-step reasoning
([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model)).
In Claude Code, set `"model": "sonnet"` in your user, project or local settings
file ([model configuration](https://code.claude.com/docs/en/model-config)). In
other assistants, use the model picker, and check which models the automatic
mode uses.

Try the smaller model first on a type of task. Go to a larger model only when
the result is not good enough. `/usage` shows the tokens of the session for each
model.

## Change the model with `/model` at the start of a task

Change the model for one task with `/model`, at the start of the task and after
`/clear`. Each model has its own cache, so a change in the middle of a task reads
the full conversation again (see [lever 2](2-stay-in-the-cache-window.md);
[Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).

## `/model` also changes your default

`/model` also saves your choice as the default for new sessions, in your user
settings
([Claude Code docs](https://code.claude.com/docs/en/model-config#setting-your-model)).
To switch for this session only, open `/model`, choose the model and press `s`.

If the project or local settings file also has a `model` key, that key wins over
your user settings. The next session then starts on the project model, whatever
you saved with `/model`.

## Use `opusplan` for one plan and a long work phase

`opusplan` uses Opus in plan mode and Sonnet for the work
([model aliases](https://code.claude.com/docs/en/model-config#model-aliases)).
Each switch between plan mode and work mode starts a new cache. It fits one plan
followed by a long work phase, and it costs more if you switch often.

## Give simple subagents a small model

For simple subagent work such as running tests, set `model: haiku` in the
subagent file (see [lever 1](1-keep-the-context-short.md)).

---

[← 2 · Stay in the cache window](2-stay-in-the-cache-window.md) · [All levers](../README.md) · [4 · Lower the effort →](4-lower-the-effort.md)
