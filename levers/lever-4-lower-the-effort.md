[← Lever 3: Use a smaller model](lever-3-use-a-smaller-model.md) &emsp; [All levers](../README.md) &emsp; [Lever 5: Make fewer round trips →](lever-5-make-fewer-round-trips.md)

# Lever 4: Lower the effort

*Reading time: about 8 minutes.*

> [!IMPORTANT]
> Use a low or medium effort level for most coding tasks, and go up only for the hard ones.

**Why it matters:** The effort level sets how much the model thinks before it
answers. Thinking is the reasoning that the model writes before its answer. It is
not the answer itself. Each token of thinking uses energy, like each token of the
answer. You often cannot read this reasoning. Other labs copy the
reasoning of a model to train their own models, which is called distillation, and
Anthropic summarizes or hides it to stop this
([Anthropic](https://www.anthropic.com/news/distillation-defenses)). But the
hidden tokens still use energy and you still pay for them. On the newest Claude
models you cannot turn thinking off, so the effort level is your control. The Claude Code docs say: "Lower effort is faster and cheaper for
straightforward tasks"
([Claude Code docs](https://code.claude.com/docs/en/model-config#choose-an-effort-level)).

What the docs and the tests say:
- The effort level changes every output token: the thinking, the text of the
  answer and the tool calls. A lower level also gives fewer and shorter tool calls.
  The model follows the level as a guide, not as a limit. The docs call it "a
  behavioral signal, not a strict token budget". At a low level, the model still
  thinks about a hard problem, but it thinks less
  ([Claude API docs](https://platform.claude.com/docs/en/build-with-claude/effort#how-effort-works)).
- You pay for all the thinking, also the part that you cannot read. The API bills
  thinking tokens as output tokens, even when it returns only a summary or an empty
  block ([Claude API docs](https://platform.claude.com/docs/en/build-with-claude/thinking)).
  Claude Code shows a collapsed stub, and the default can be tens of thousands of
  tokens for each request
  ([Claude Code docs](https://code.claude.com/docs/en/costs#adjust-extended-thinking)).
- Tests on open models show a large energy cost. On average, reasoning models use
  30 times more energy than other models. For the same model, three open models
  used 150 to 700 times more energy with reasoning on than off. The authors say
  that the number of output tokens explains this "in large part"
  ([AI Energy Score v2](https://huggingface.co/blog/sasha/ai-energy-score-v2)).
- A second test compares two kinds of task, not a switch. A problem-solving task,
  with reasoning on, used 25 times more energy for each response than a text
  conversation. Its responses had 10 times more output tokens
  ([ML.ENERGY](https://ml.energy/blog/measurement/energy/diagnosing-inference-energy-consumption-with-the-mlenergy-leaderboard-v30/)).

<img alt="Two horizontal bars, each in a frame. At a low effort level, the model writes a short thinking part and then the answer. At a high effort level, the model writes a much longer thinking part and then an answer of the same size. The thinking and the answer are both output tokens." src="../images/more-thinking-more-output.svg#gh-light-mode-only">
<img alt="Two horizontal bars, each in a frame. At a low effort level, the model writes a short thinking part and then the answer. At a high effort level, the model writes a much longer thinking part and then an answer of the same size. The thinking and the answer are both output tokens." src="../images/more-thinking-more-output-dark.svg#gh-dark-mode-only">

These tests used open models, not Claude models. This guide has no energy figure
for Claude, so the size of the saving for Claude is not known. The Claude Code docs
confirm that you cannot turn thinking off on Opus 5.5, Sonnet 5.5, Haiku 5.5 and
the Fable models
([Claude Code docs](https://code.claude.com/docs/en/model-config#extended-thinking)).

| Item | Role |
|---|---|
| [4.1 Use a low or medium level for most coding tasks and go up only when needed](#41-use-a-low-or-medium-level-for-most-coding-tasks-and-go-up-only-when-needed) | Use |
| [4.2 Use `ultrathink` for one hard turn instead of raising the level](#42-use-ultrathink-for-one-hard-turn-instead-of-raising-the-level) | Use for one turn |
| [4.3 Set a default level, and press `s` to change it for one session only](#43-set-a-default-level-and-press-s-to-change-it-for-one-session-only) | Set your default |
| [4.4 Give simple subagent tasks a low effort](#44-give-simple-subagent-tasks-a-low-effort) | Use a low effort for subagents |

## 4.1 Use a low or medium level for most coding tasks and go up only when needed

**What:** Start each task at `low` or `medium`. Go up one level only when the
result is not good enough.

**Why:** "Each level trades token spend against capability"
([Claude Code docs](https://code.claude.com/docs/en/model-config#choose-an-effort-level)).
The docs say that `low` gives "significant token savings with some capability
reduction", and that `medium` gives moderate savings
([Claude API docs](https://platform.claude.com/docs/en/build-with-claude/effort#effort-levels)).
Most coding tasks do not need deep reasoning. The default level depends on the
model. It is `medium` on Fable 5.2, Opus 5.5, Sonnet 5.5 and Haiku 5.5. It is
`xhigh` on Opus 4.7, and `high` on the other models that support effort. A saved
level, a project setting or a variable can change it (see 4.3).

The levels are `low`, `medium`, `high`, `xhigh` and `max`. Opus 4.6 and Sonnet 4.6
have no `xhigh`. A model that does not support effort has no levels
([Claude Code docs](https://code.claude.com/docs/en/model-config#adjust-effort-level)).
The Claude Code docs suggest these levels
([Claude Code docs](https://code.claude.com/docs/en/model-config#choose-an-effort-level)):

| Level | The docs suggest it for |
|---|---|
| `low` | Quick exchanges where you review each result, or a small change such as a rename |
| `medium` | Day-to-day engineering with a clear scope, such as a new feature |
| `high` | Work where checks matter or edge cases are likely, such as a bug in existing code |
| `xhigh` | Deeper reasoning, at a higher token cost |
| `max` | Hard problems that you want Claude to work through alone |

**How:**
1. Check the level that your session uses now (see Where).
2. Choose the level before the first message of a task. A change in the middle of
   a task can read the whole conversation again, with no cache (see
   [2.5](lever-2-stay-in-the-cache-window.md#25-change-the-effort-level-the-speed-and-other-settings-only-between-tasks)).
3. Go up one level only when the result is not good enough. To check the level on
   your own code, run the same small task at two levels and compare the results.

**Where:**
- Terminal: `/effort status` prints the level. `/effort` with no argument opens a
  slider, and you choose the level there
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#set-the-effort-level)).
  See 4.3 for the difference between one session and a saved default.
- Desktop app, Code tab: Cmd+Shift+E opens the effort menu, with the levels `Low`,
  `Medium`, `High`, `Extra` and `Max`. On Windows and Linux, press Ctrl instead of
  Cmd
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#keyboard-shortcuts)).

**Watch out:**
- Test `max` before you use it often. The docs say that it "may show diminishing
  returns and is prone to overthinking"
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#choose-an-effort-level)).
- For Opus 5, effort controls the volume of thinking. Changing it does not
  reliably shorten the visible answer
  ([Claude API docs](https://platform.claude.com/docs/en/build-with-claude/effort)).
- The effort menu also has an **Ultracode** switch. It is not an effort level and it
  does not lower the effort. With it on, Claude plans a workflow for each
  substantive task, at the level that your session uses
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#adjust-effort-level)).
  This guide has no measure of its token cost, so keep it off unless you want
  workflows.
- A level that is too low can cost retries. A wrong result at `low` often needs
  corrections, and each correction sends the whole conversation again (see
  [lever 5](lever-5-make-fewer-round-trips.md)). If the result is wrong at a low
  level, go up one level for a new attempt and do not send corrections. This guide
  has no measure of where the break-even is.
- The docs here cover Claude only. In other assistants, look for a setting named
  effort, thinking or reasoning.

## 4.2 Use `ultrathink` for one hard turn instead of raising the level

**What:** Keep your session at `low` or `medium`. For one hard turn, write the
word `ultrathink` in the prompt.

**Why:** The word asks for deeper reasoning on that turn, "without changing your
session effort setting"
([Claude Code docs](https://code.claude.com/docs/en/model-config#use-ultrathink-for-one-off-deep-reasoning)).
So a high level does not stay on for the easy tasks that come after the hard one.

**How:** Write the word `ultrathink` anywhere in the prompt of the hard turn. It
is not a command: do not start it with `/`. Claude Code reads the word in the text
of your prompt. It needs Claude Code 2.1.311 or later. In the terminal, the word
can also raise that turn to `xhigh`. Phrases such as "think hard" are ordinary
text and have no effect of this kind.

**Where:** In the terminal and in the desktop app, Code tab, write the word in
the prompt. The docs for the desktop app do not describe it, so check the result
on a test prompt.

**Watch out:** On some providers, such as Amazon Bedrock, the word cannot raise
the turn. Set the effort level instead.

## 4.3 Set a default level, and press `s` to change it for one session only

**What:** Set `effortLevel` in the settings of a project. New sessions in the
project then start at a level that fits it. To change the level for one session
only, press `s` in the `/effort` slider, and do not press Enter.

**Why:**
- Enter saves the level as your default for the model, and it applies in all later
  sessions. A high level that you chose for one hard bug then stays on for every
  later session on that model. The `s` key applies the level to this session only
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#set-the-effort-level)).
- Claude Code takes the first level that applies in the order of the figure
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#adjust-effort-level)).

<img alt="Five rows from the strongest to the weakest way to set the effort level: the CLAUDE_CODE_EFFORT_LEVEL variable, a level that you pick in the session with /effort or the /model picker, the --effort option, a saved level or the effortLevel setting, and the default level of the model." src="../images/effort-setting-priority.svg#gh-light-mode-only">
<img alt="Five rows from the strongest to the weakest way to set the effort level: the CLAUDE_CODE_EFFORT_LEVEL variable, a level that you pick in the session with /effort or the /model picker, the --effort option, a saved level or the effortLevel setting, and the default level of the model." src="../images/effort-setting-priority-dark.svg#gh-dark-mode-only">

A top-level `effortLevel` in the project, local or managed settings applies to
every model. In your user file `~/.claude/settings.json`, it does not count for
Opus 5.5
([Claude Code docs](https://code.claude.com/docs/en/model-config#adjust-effort-level)).

**How:**
- For a project, add `"effortLevel": "medium"` to `.claude/settings.json` (shared
  with everyone on the project) or to `.claude/settings.local.json` (only for
  you)
  ([settings reference](https://code.claude.com/docs/en/settings-reference#effortlevel)).
  The values are `low`, `medium`, `high` and `xhigh`.
- For all your projects, run `/effort medium`. Claude Code saves the level for the
  current model.
- For one session only:
  1. Run `/effort` with no argument. The slider opens. In the `/model` picker, the
     same slider is next to the model.
  2. Move the slider to the level that you want.
  3. Press `s`. Do not press Enter.

A level that you type after the command, for example `/effort high`, is saved in
the same way as Enter. Claude Code saves the level for each model, under `modelSettings` in your user settings. It needs Claude Code
2.1.257 or later. To clear a level that you saved, run `/effort auto`. The level
`max` applies to the current session only, whatever key you use.

**Where:**
- Terminal: the settings files and the slider above.
- Desktop app, Code tab: the docs do not describe an `s` key in the effort menu
  (Cmd+Shift+E). Check in the menu how long your choice lasts.

**Watch out:** `CLAUDE_CODE_EFFORT_LEVEL` beats `/effort`, `--effort` and the
settings. If `/effort` seems to do nothing, check that this variable is not set
([Claude Code docs](https://code.claude.com/docs/en/env-vars)).

## 4.4 Give simple subagent tasks a low effort

**What:** In the file of a subagent that does simple work, such as running tests
or a search, set `effort: low`.

**Why:** A subagent that has no `effort` runs at the level of your session. If
your session is at a high level, the subagent thinks as much as the main agent.
On another model than the main one, the level of your session does not apply
([Claude Code docs](https://code.claude.com/docs/en/model-config#effort-level-of-subagents-and-teammates)).
The `effort` field in the file of the subagent overrides the level of the session
([Claude Code docs](https://code.claude.com/docs/en/sub-agents#supported-frontmatter-fields)).

**How:** Add the field to the frontmatter of the subagent file, next to `model`
(see [3.4](lever-3-use-a-smaller-model.md#34-give-simple-subagent-tasks-to-haiku)).
For example:

```markdown
---
name: test-runner
description: Runs the tests and reports the failures
model: haiku
effort: low
---
```

The options are `low`, `medium`, `high`, `xhigh` and `max`. The levels that you can
use depend on the model. To write a subagent, see the
[Claude Code docs](https://code.claude.com/docs/en/sub-agents). For the reason to
use a subagent, see [1.3](lever-1-keep-the-context-short.md#13-give-noisy-work-to-a-subagent).

**Watch out:** `CLAUDE_CODE_EFFORT_LEVEL` also beats the `effort` field of a
subagent (see 4.3).

---

[← Lever 3: Use a smaller model](lever-3-use-a-smaller-model.md) &emsp; [All levers](../README.md) &emsp; [Lever 5: Make fewer round trips →](lever-5-make-fewer-round-trips.md)
