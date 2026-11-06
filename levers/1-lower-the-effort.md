# 1 · Lower the effort

**In one line:** use a low effort level for simple tasks, and keep a high level
for hard ones.

## Why it matters

The effort level sets how much the model thinks before it answers. Thinking is
text that the model generates, and generated text is what uses the most energy.

- Reasoning models use on average **30 times more energy** than models without
  reasoning. The same model uses **150 to 700 times more energy** with reasoning
  on, because it generates 300 to 800 times more tokens
  ([AI Energy Score v2](https://huggingface.co/blog/sasha/ai-energy-score-v2)).
- In Claude Code, thinking tokens are billed as output tokens, and the default
  can be tens of thousands of tokens for each request
  ([Claude Code docs](https://code.claude.com/docs/en/costs#adjust-extended-thinking)).
- On the newest Claude models (Opus 5.5, Sonnet 5.5, Haiku 5.5, Fable), you
  cannot turn thinking off. The effort level is the control that you have.

Most coding tasks do not need deep reasoning: a rename, a small fix, a question
about the code, a test to add.

## How to apply

**Claude Code**
- Set a default: `"effortLevel": "medium"` in your settings (see below).
- Change it for one task: `/effort low` for small tasks, `/effort high` for a
  hard bug or a design decision.
- Choose the level at the start of a task. On some models, a change in the
  middle of a session restarts the cache
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#changing-effort-level)).

**Other assistants:** look for a setting named effort, thinking, reasoning or
"fast / thinking" mode. Use the fast mode by default.

**Teams:** put the default in the project settings file
(`.claude/settings.json`), which you commit with the code. Then every person
starts with the same level.

## Copy this

In `~/.claude/settings.json` (for you) or `.claude/settings.json` (for the
project):

```json
{
  "effortLevel": "medium"
}
```

The values are `low`, `medium`, `high` and `xhigh`
([settings reference](https://code.claude.com/docs/en/settings-reference#effortlevel)).

## Check it

- `/usage` in Claude Code shows the tokens of the current session. Compare the
  output tokens of a small task at two effort levels.
- If the answers become worse, go up one level for that type of task only.

## Resources

- [AI Energy Score v2](https://huggingface.co/blog/sasha/ai-energy-score-v2):
  energy measurements with reasoning on and off.
- [AI Energy Score leaderboard](https://huggingface.co/spaces/AIEnergyScore/Leaderboard):
  compares the energy of models for the same tasks.
- [Adjust the effort level](https://code.claude.com/docs/en/model-config#adjust-effort-level)
  (Claude Code docs).

---

[All levers](../README.md) · Next: [2 · Use a smaller model](2-use-a-smaller-model.md)
