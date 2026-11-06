# 4 · Use only the AI you need

**Space:** How you choose and use AI
**Principle:** 🌱 Eco-friendly · *Is it necessary?*

## Why it matters

Each request to a model uses energy and hardware in a data centre. The cost is
not the same for all models: in the AI Energy Score measurements, reasoning
models used on average 30 times more energy than models without reasoning
([AI Energy Score v2](https://huggingface.co/blog/sasha/ai-energy-score-v2)).
Long agent sessions send a large context (all the text that the model reads: your prompt, files and the history of the session) again with each step.

## What to do

### Everyone

- First ask: does this task need AI? A search, the documentation or a template is often enough.
- Start a new session for each new task.
- Stop the agent when it goes round in circles. Change your request in place of trying again and again.

### Developers

- Choose the smallest model that does the task well. Use reasoning modes (the model works through many steps before it answers) only for hard problems.
- Keep large files that are not related to the task out of the context.
- Use tools without AI for mechanical tasks: linters, formatters, code generators, refactoring tools.

### Teams and architects

- Make a small model the default for routine tasks, and large models a choice for hard tasks.
- Track AI use for each team. The cost in tokens is a rough indicator of the energy used.
- Prefer providers that publish data on their energy use and emissions.

## Resources

- [AI Energy Score leaderboard](https://huggingface.co/spaces/AIEnergyScore/Leaderboard):
  compares the energy use of models for the same tasks.
- [EcoLogits](https://ecologits.ai/): estimates the energy and environmental
  impact of requests to AI providers (Python library).

## Not covered here

- Code and features that AI makes too easy to add: see [9 · Lean code](9-lean-code.md).
- The efficiency of the provider's data centres: you cannot control it, but you can ask providers to publish their data.

---

Previous: [3 · Watch for bias](3-watch-for-bias.md) · [Index](../README.md) · Next: [5 · Stay in control](5-stay-in-control.md)
