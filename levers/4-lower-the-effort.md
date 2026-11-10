[← 3 Use a smaller model](3-use-a-smaller-model.md) &emsp; [All levers](../README.md) &emsp; [5 Make fewer round trips →](5-make-fewer-round-trips.md)

# 4 Lower the effort

**In one line:** use a low effort level for simple tasks, and keep a high level
for hard ones.

## 4.1 Thinking uses most of the energy

The effort level sets how much the model thinks before it answers. Thinking is
text that the model generates, and generated text uses most of the energy.

Across many models, reasoning models use on average 30 times more energy than
models without reasoning. For a single model the gap is larger: three open
models used 150 to 700 times more energy with reasoning on than off, mostly
because they generated 300 to 800 times more tokens
([AI Energy Score v2](https://huggingface.co/blog/sasha/ai-energy-score-v2)).

## 4.2 On the newest Claude models, effort is your only control

On Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.1, you cannot turn thinking off.
The effort level is the control that you have. In Claude Code, thinking tokens
are billed as output tokens, and the default can be tens of thousands of tokens
for each request
([Claude Code docs](https://code.claude.com/docs/en/costs#adjust-extended-thinking)).

## 4.3 Use a low level for most coding tasks

Most coding tasks do not need deep reasoning. Examples are a rename, a small
fix, a question about the code and a test to add. Use `low` for small tasks and
`high` for a hard bug or a design decision. If the answers become worse, go up
one level for that type of task only. `/usage` shows the tokens of the current
session, so you can compare the output tokens of a small task at two levels.

In other assistants, look for a setting named effort, thinking or reasoning, or
for a switch between an "instant" mode and a "thinking" mode. Use the mode
without reasoning by default.

## 4.4 Check the default level

Opus 5.5, Sonnet 5.5 and Haiku 5.5 start at `medium`. Most other models start at
`high`, and Opus 4.7 at `xhigh`
([Claude Code docs](https://code.claude.com/docs/en/model-config#adjust-effort-level)).
The values are `low`, `medium`, `high` and `xhigh`.

## 4.5 Change the level for one task with `/effort`

Open `/effort`, choose the level and press `s`. The level then applies to this
session only. A level that you type after `/effort` (for example `/effort high`)
becomes your default for later sessions, so check what you typed.

## 4.6 Set a default level for a project

Set `effortLevel` in `.claude/settings.json` (for everyone on the project) or in
`.claude/settings.local.json` (only for you). In these two files, the key
applies to every model. Do not put it in `~/.claude/settings.json`: there,
Opus 5.5 and later models ignore it
([settings reference](https://code.claude.com/docs/en/settings-reference#effortlevel)).
For all your projects, use `/effort medium` once instead: Claude Code saves it
for the current model.

---

[← 3 Use a smaller model](3-use-a-smaller-model.md) &emsp; [All levers](../README.md) &emsp; [5 Make fewer round trips →](5-make-fewer-round-trips.md)
