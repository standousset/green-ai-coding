[All levers](../README.md) · [2 · Use a smaller model →](2-use-a-smaller-model.md)

# 1 · Lower the effort

**In one line:** use a low effort level for simple tasks, and keep a high level
for hard ones.

## Why it matters

The effort level sets how much the model thinks before it answers. Thinking is
text that the model generates, and generated text uses most of the energy.

- Across many models, reasoning models use on average 30 times more energy than
  models without reasoning. For a single model the gap is larger: three open
  models used 150 to 700 times more energy with reasoning on than off, mostly
  because they generated 300 to 800 times more tokens
  ([AI Energy Score v2](https://huggingface.co/blog/sasha/ai-energy-score-v2)).
- In Claude Code, thinking tokens are billed as output tokens, and the default
  can be tens of thousands of tokens for each request
  ([Claude Code docs](https://code.claude.com/docs/en/costs#adjust-extended-thinking)).
- On the newest Claude models (Opus 5.5, Sonnet 5.5, Haiku 5.5, Fable 5.1), you
  cannot turn thinking off. The effort level is the control that you have.

Most coding tasks do not need deep reasoning. Examples are a rename, a small
fix, a question about the code and a test to add.

## How to apply

**Claude Code**
- Check the default. Opus 5.5, Sonnet 5.5 and Haiku 5.5 start at `medium`. Most
  other models start at `high`, and Opus 4.7 at `xhigh`
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#adjust-effort-level)).
- A level that you type after `/effort` (for example `/effort high`) becomes your
  default for later sessions, so check what you typed.
- To change the level for one task only, open `/effort`, choose the level and
  press `s`. The level then applies to this session only. Use `low` for small
  tasks and `high` for a hard bug or a design decision.
- Set a default for a project in the project or local settings file (see below).
- Choose the level at the start of a task. On Opus 5.5, Sonnet 5.5, Haiku 5.5
  and Fable 5.1, a change in the middle of a session keeps the cache if you use
  an API key or a Claude subscription. On other models, and on Amazon Bedrock
  and Google Cloud, it restarts the cache
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#changing-effort-level)).

**Other assistants:** look for a setting named effort, thinking or reasoning, or
for a switch between an "instant" mode and a "thinking" mode. Use the mode
without reasoning by default.

**Teams:** put the default in the project settings file (in Claude Code:
`.claude/settings.json`), which you commit with the code. Then every person
starts with the same level.

## Copy this

Add this key to `.claude/settings.json` in your project (for everyone on the
project) or to `.claude/settings.local.json` (only for you):

```json
{
  "effortLevel": "medium"
}
```

The values are `low`, `medium`, `high` and `xhigh`. In these two files, the key
applies to every model. Do not put it in `~/.claude/settings.json`: there, Opus
5.5 and later models ignore it
([settings reference](https://code.claude.com/docs/en/settings-reference#effortlevel)).
For all your projects, use `/effort medium` once instead: Claude Code saves it
for the current model.

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

[All levers](../README.md) · [2 · Use a smaller model →](2-use-a-smaller-model.md)
