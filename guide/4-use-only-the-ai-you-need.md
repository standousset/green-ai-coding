# 4 · Use only the AI you need

**Space:** How you choose and use AI
**Principle:** 🌱 Eco-friendly · *Is it necessary?*

This is the main part of the guide: how to code with AI in a green way.

## What makes the impact

Each request to a coding assistant uses energy in a data centre. Four things
decide how much, and how much CO₂ it emits:

| What | Why it counts |
|---|---|
| **Tokens generated** | The model's energy grows with each token that it writes ([EcoLogits method](https://ecologits.ai/latest/methodology/llm_inference/)). Thinking tokens are generated tokens too. |
| **Model size** | The energy for each generated token grows with the number of active parameters of the model ([EcoLogits method](https://ecologits.ai/latest/methodology/llm_inference/)). |
| **Tokens read** | The assistant sends the full conversation again with every request ([Claude Code docs](https://code.claude.com/docs/en/costs)). Prompt caching avoids part of this work. |
| **Electricity** | The same energy emits 28 to 670 gCO₂ per kWh, depending on the country (2025, [Our World in Data / Ember](https://ourworldindata.org/grapher/carbon-intensity-electricity)). |

You control the first three every day. You control the fourth only when you
choose a provider or a region.

## The levers

### 1. Lower the effort for simple tasks

Thinking tokens are billed as output tokens, and the default can be tens of
thousands of tokens for each request
([Claude Code docs](https://code.claude.com/docs/en/costs)). In the AI Energy
Score measurements, reasoning models used on average 30 times more energy than
models without reasoning
([AI Energy Score v2](https://huggingface.co/blog/sasha/ai-energy-score-v2)).

**How:** use a low effort level for renames, small fixes and questions, and keep
high effort for design and hard bugs. In Claude Code: `/effort`. In other tools,
look for an effort, thinking or reasoning setting.

### 2. Use the smallest model that does the task

A larger model uses more energy for each token that it generates. The Claude
Code documentation says that Sonnet handles most coding tasks well, and that
Opus is for complex architecture and multi-step reasoning
([Claude Code docs](https://code.claude.com/docs/en/costs)).

**How:** make a mid-size model your default, and choose the largest model only
for the hardest tasks. In Claude Code: `/model`, or a default in `/config`.

### 3. Keep the context small

The full conversation is sent again with every request, so old, unrelated
context costs something each time. A long context also makes answers less
reliable: in a study of 18 models, performance became less reliable as the
input got longer ([Chroma, Context Rot](https://www.trychroma.com/research/context-rot)).

**How:**
- Start a new session for each new task. In Claude Code: `/clear`.
- Give only the part of a log or a file that matters.
- Keep your rules file (`AGENTS.md`, `CLAUDE.md`) short: it is read with every request.
- Summarise a long session before you continue. In Claude Code: `/compact`.
- Check what fills the context. In Claude Code: `/context`.

### 4. Stay inside the cache window

Prompt caching reuses the work already done on the start of the conversation.
On the Anthropic API, a cache read costs about 0.1 times the normal input
price. The cache lasts 5 minutes by default, each use starts the 5 minutes
again, and each model has its own cache
([Anthropic](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)).
After a long break, the next request processes the full context again
([Claude Code docs](https://code.claude.com/docs/en/costs)).

**How:**
- Change the model between tasks (after `/clear`), not in the middle of a task.
- After a long break, start a new session with a short summary, if the old
  context is not needed.

### 5. Turn off the tools that you do not use

Each connected tool server (MCP server) adds to the context. The Claude Code
documentation says that command-line tools such as `gh` are more
context-efficient than MCP servers
([Claude Code docs](https://code.claude.com/docs/en/costs)).

**How:** disable unused servers (in Claude Code: `/mcp`), and prefer
command-line tools when they exist.

### 6. Send noisy work to a small subagent

Test runs, logs and documentation searches fill the context fast. A subagent
does this work in its own context and sends back only a summary
([Claude Code docs](https://code.claude.com/docs/en/costs)). It still makes
its own requests, so give it a small model.

**How:** in Claude Code, set `model: haiku` in the subagent configuration for
simple subagent tasks.

### 7. Ask precisely, and plan first

Each failed attempt is a full extra round of requests. A specific request with
the files, the constraints and the expected result needs fewer rounds.

**How:** for a large task, ask for a plan first, correct it, then let the
assistant work. Do not say "try again" without new information.

### 8. Do not use AI when a tool does the job

Search, rename, formatting and code generation from templates do not need a
model. A linter, a formatter or your editor's refactoring tools do these tasks
with almost no energy.

### When you can choose: a low-carbon region

The electricity of a data centre emits from about 28 gCO₂/kWh (Norway) and 41
(France) to 384 (United States), 589 (Poland) and 670 (India) (2025,
[Our World in Data / Ember](https://ourworldindata.org/grapher/carbon-intensity-electricity)).
Most coding assistants do not let you choose where your requests run. When you
use a model through a cloud provider that lets you choose a region, choose one
with low-carbon electricity.

## For teams

- Set a mid-size model and a moderate effort level as the team default, and make
  the largest model a choice for hard tasks.
- Keep the shared rules file short, and review it like code.
- Track AI use for each team. The cost in tokens is a rough indicator of the
  energy used.
- Prefer providers that publish data on their energy use and emissions.

## Resources

- [Prompts for this part](../toolkit/prompts.md#4--use-only-the-ai-you-need): ready to copy into your assistant.
- [Manage costs effectively](https://code.claude.com/docs/en/costs) (Claude Code):
  the official list of ways to reduce token use. Most of them also reduce energy.
- [AI Energy Score](https://huggingface.co/spaces/AIEnergyScore/Leaderboard):
  compares the energy use of models for the same tasks.
- [EcoLogits](https://ecologits.ai/): estimates the energy and emissions of
  requests to AI providers, and explains the method.
- [Carbon intensity of electricity](https://ourworldindata.org/grapher/carbon-intensity-electricity)
  (Our World in Data, based on Ember): values for each country.

## Not covered here

- Code and features that AI makes too easy to add: see [9 · Lean code](9-lean-code.md).
- The efficiency of the provider's data centres: you cannot control it, but you
  can ask providers to publish their data.

---

Previous: [3 · Watch for bias](3-watch-for-bias.md) · [Index](../README.md) · Next: [5 · Stay in control](5-stay-in-control.md)
