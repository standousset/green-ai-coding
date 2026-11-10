[← 3 Use a smaller model](3-use-a-smaller-model.md) &emsp; [All levers](../README.md) &emsp; [5 Make fewer round trips →](5-make-fewer-round-trips.md)

# 4 Lower the effort

### In one line: use a low or medium effort level for most coding tasks, and go up only for the hard ones.

**Why it matters:** The effort level sets how much the model thinks before it
answers. Thinking is text that the model writes, and each token that it writes
uses energy (see 4.1). On the newest Claude models you cannot turn thinking off,
so the effort level is the control that you have. The Claude Code docs say:
"Lower effort is faster and cheaper for straightforward tasks"
([Claude Code docs](https://code.claude.com/docs/en/model-config#choose-an-effort-level)).

| Item | Role |
|---|---|
| [4.1 Know that more thinking means more tokens and more energy](#41-know-that-more-thinking-means-more-tokens-and-more-energy) | Learn the rule |
| [4.2 Check your effort level now](#42-check-your-effort-level-now) | Learn your level |
| [4.3 Use a low or medium level for most coding tasks and go up only when needed](#43-use-a-low-or-medium-level-for-most-coding-tasks-and-go-up-only-when-needed) | Use |
| [4.4 Use `ultrathink` for one hard turn instead of raising the level](#44-use-ultrathink-for-one-hard-turn-instead-of-raising-the-level) | Use for one turn |
| [4.5 Press `s` to change the level for this session only](#45-press-s-to-change-the-level-for-this-session-only) | Protect your default |
| [4.6 Set a default level for a project](#46-set-a-default-level-for-a-project) | Set your default |
| [4.7 Give simple subagent tasks a low effort](#47-give-simple-subagent-tasks-a-low-effort) | Use a low effort for subagents |

Each item below has the same parts:

- The **title** is the recommendation.
- **What** to do.
- **Why** it saves energy.
- **How** to do it.
- **Where** it applies. This lists the three ways to use Claude: the terminal
  (Claude Code CLI), the Code tab of the desktop app, and chat.
- **Watch out**: some items add this.

If an item does not list a way, the docs do not describe it for that way: check
in the app.

## 4.1 Know that more thinking means more tokens and more energy

**What:** Treat the effort level as a cost. A higher level means more tokens, and
more tokens use more energy.

**Why:**
- Effort changes all the tokens of the response: the text, the tool calls and the
  thinking. The docs call it a signal, not a strict token budget
  ([Claude API docs](https://platform.claude.com/docs/en/build-with-claude/effort#how-effort-works)).
- You pay for the thinking. Claude Code bills thinking tokens as output tokens,
  and the default can be tens of thousands of tokens for each request
  ([Claude Code docs](https://code.claude.com/docs/en/costs#adjust-extended-thinking)).
- Tests on open models show a large energy cost. On average, reasoning models use
  30 times more energy than other models. For the same model, three open models
  used 150 to 700 times more energy with reasoning on than off. The authors say
  that the number of output tokens explains this "in large part"
  ([AI Energy Score v2](https://huggingface.co/blog/sasha/ai-energy-score-v2)).
- A second test compares tasks, not a switch. Problem solving, with reasoning on,
  used 25 times more energy for each response than a text conversation. It wrote
  10 times more output tokens
  ([ML.ENERGY](https://ml.energy/blog/measurement/energy/diagnosing-inference-energy-consumption-with-the-mlenergy-leaderboard-v30/)).

![Two horizontal bars, each in a frame. At a low effort level, the model writes a short thinking part and then the answer. At a high effort level, the model writes a much longer thinking part and then an answer of the same size. The thinking and the answer are both output tokens.](../images/more-thinking-more-output.svg)

**How:** Choose the level with the items below.

**Where:** This applies to all three ways to use Claude.

**Watch out:**
- These tests used open models, not Claude models. This guide has no energy
  figure for Claude, so the size of the saving for Claude is not known.
- You cannot turn thinking off on Opus 5.5, Sonnet 5.5, Haiku 5.5 and the Fable
  models. The effort level is your control there
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#extended-thinking)).
  In chat, the thinking switch does not appear for a model that always thinks
  ([Claude help center](https://support.claude.com/en/articles/10574485-using-extended-thinking)).

## 4.2 Check your effort level now

**What:** Find out which level your sessions start on.

**Why:** The default depends on the model. If you never set a level, you may use
a higher level than your tasks need.

| Model | Default level |
|---|---|
| Fable 5.2, Opus 5.5, Sonnet 5.5, Haiku 5.5 | `medium` |
| Opus 4.7 | `xhigh` |
| Other models in the effort table | `high` |

The levels are `low`, `medium`, `high`, `xhigh` and `max`. Opus 4.6 and Sonnet 4.6
have no `xhigh`. A model that is not in the table does not support effort
([Claude Code docs](https://code.claude.com/docs/en/model-config#adjust-effort-level)).

**How:** Open the effort control in the list below. A saved level, a project
setting or a variable can change the default. The order is in 4.6.

**Where:**
- Terminal: `/effort` with no argument opens a slider
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#set-the-effort-level)).
- Desktop app, Code tab: Cmd+Shift+E opens the effort menu. On Windows and Linux,
  press Ctrl instead of Cmd
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#keyboard-shortcuts)).
- Chat: click the model name next to the send button, then **Effort**. The
  selector exists for the current models, for example Sonnet 5.5 and Opus 5.5
  ([Claude help center](https://support.claude.com/en/articles/10574485-using-extended-thinking)).

## 4.3 Use a low or medium level for most coding tasks and go up only when needed

**What:** Start each task at `low` or `medium`. Go up one level only when the
result is not good enough.

**Why:** "Each level trades token spend against capability"
([Claude Code docs](https://code.claude.com/docs/en/model-config#choose-an-effort-level)).
The docs say that `low` gives "significant token savings with some capability
reduction", and that `medium` gives moderate savings
([Claude API docs](https://platform.claude.com/docs/en/build-with-claude/effort#effort-levels)).
Most coding tasks do not need deep reasoning.

The Claude Code docs suggest these levels
([Claude Code docs](https://code.claude.com/docs/en/model-config#choose-an-effort-level)):

| Level | The docs suggest it for |
|---|---|
| `low` | Quick exchanges where you review each result, or a small change such as a rename |
| `medium` | Day-to-day engineering with a clear scope, such as a new feature |
| `high` | Work where checks matter or edge cases are likely, such as a bug in existing code |
| `xhigh` | Deeper reasoning, at a higher token cost |
| `max` | Hard problems that you want Claude to work through alone |

**How:** Choose the level before the first message of a task. A change in the
middle of a task can read the whole conversation again, with no cache (see
[2.5](2-stay-in-the-cache-window.md#25-change-the-effort-level-in-the-middle-of-a-task-only-on-a-model-that-keeps-the-cache)).
To check the level on your own code, run the same small task at two levels and
compare the results.

**Where:**
- Terminal: `/effort`, then choose the level. See 4.5 for the difference between
  one session and a saved default.
- Desktop app, Code tab: Cmd+Shift+E opens the effort menu.
- Chat: **Effort** in the model menu. A higher level uses more tokens, so you
  reach your usage limit faster
  ([Claude help center](https://support.claude.com/en/articles/10574485-using-extended-thinking)).

**Watch out:**
- Test `max` before you use it often. The docs say that it "may show diminishing
  returns and is prone to overthinking"
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#choose-an-effort-level)).
- For Opus 5, effort controls the volume of thinking. Changing it does not
  reliably shorten the visible answer
  ([Claude API docs](https://platform.claude.com/docs/en/build-with-claude/effort)).
- The docs here cover Claude only. In other assistants, look for a setting named
  effort, thinking or reasoning.

## 4.4 Use `ultrathink` for one hard turn instead of raising the level

**What:** Keep your session at `low` or `medium`. For one hard turn, write the
word `ultrathink` in the prompt.

**Why:** The word asks for deeper reasoning on that turn, "without changing your
session effort setting"
([Claude Code docs](https://code.claude.com/docs/en/model-config#use-ultrathink-for-one-off-deep-reasoning)).
So a high level does not stay on for the easy tasks that come after the hard one.

**How:** Include `ultrathink` anywhere in the prompt. It needs Claude Code 2.1.311
or later. In the terminal, the word can also raise that turn to `xhigh`. Phrases
such as "think hard" are ordinary text and have no effect of this kind.

**Where:**
- Terminal: the word in the prompt, as above.

**Watch out:** On some providers, such as Amazon Bedrock, the word cannot raise
the turn. Set the effort level instead.

## 4.5 Press `s` to change the level for this session only

**What:** When you choose a level in the `/effort` slider or in the `/model`
picker, press `s`. Do not press Enter.

**Why:** Enter saves the level as your default and applies it in later sessions.
A level that you type after `/effort`, for example `/effort high`, is saved in the
same way. The level is saved for each model, under `modelSettings` in your user
settings. A high level that you chose for one hard bug then stays on for every
later session on that model. The `s` key applies the level to this session only
([Claude Code docs](https://code.claude.com/docs/en/model-config#set-the-effort-level)).

**How:** Open `/effort`, move the slider, and press `s`. It needs Claude Code
2.1.257 or later. To clear a level that you saved, run `/effort auto`. The level
`max` applies to the current session only, whatever key you use.

**Where:**
- Terminal: the slider and the key above.

## 4.6 Set a default level for a project

**What:** Set `effortLevel` in the settings of a project. New sessions in the
project then start at a level that fits it.

**Why:** Claude Code takes the first level that applies in the order below
([Claude Code docs](https://code.claude.com/docs/en/model-config#adjust-effort-level)).

![Five rows from the strongest to the weakest way to set the effort level: the CLAUDE_CODE_EFFORT_LEVEL variable, a level that you pick in the session with /effort or the /model picker, the --effort option, a saved level or the effortLevel setting, and the default level of the model.](../images/effort-setting-priority.svg)

A top-level `effortLevel` in the project, local or managed settings applies to
every model. In your user file `~/.claude/settings.json`, it does not count for
Opus 5.5
([Claude Code docs](https://code.claude.com/docs/en/model-config#adjust-effort-level)).

**How:** Add `"effortLevel": "medium"` to `.claude/settings.json` (shared with
everyone on the project) or to `.claude/settings.local.json` (only for you)
([settings reference](https://code.claude.com/docs/en/settings-reference#effortlevel)).
The values are `low`, `medium`, `high` and `xhigh`. For all your projects, use
`/effort medium` and press Enter once. Claude Code saves the level for the
current model.

**Where:**
- Terminal: the settings files above.

**Watch out:** `CLAUDE_CODE_EFFORT_LEVEL` beats `/effort`, `--effort` and the
settings. If `/effort` seems to do nothing, check that this variable is not set
([Claude Code docs](https://code.claude.com/docs/en/env-vars)).

## 4.7 Give simple subagent tasks a low effort

**What:** In the file of a subagent that does simple work, such as running tests
or a search, set `effort: low`.

**Why:** A subagent that has no `effort` runs at the level of your session. If
your session is at a high level, the subagent thinks as much as the main agent.
On another model than the main one, the level of your session does not apply
([Claude Code docs](https://code.claude.com/docs/en/model-config#effort-level-of-subagents-and-teammates)).
The `effort` field in the file of the subagent overrides the level of the session
([Claude Code docs](https://code.claude.com/docs/en/sub-agents#supported-frontmatter-fields)).

**How:** Add the field to the frontmatter of the subagent file, next to `model`
(see [3.7](3-use-a-smaller-model.md#37-give-simple-subagent-tasks-to-haiku)).
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
use a subagent, see [1.3](1-keep-the-context-short.md#13-give-noisy-work-to-a-subagent).

**Where:**
- Terminal: the subagent file above.

**Watch out:** `CLAUDE_CODE_EFFORT_LEVEL` also beats the `effort` field of a
subagent (see 4.6).

---

[← 3 Use a smaller model](3-use-a-smaller-model.md) &emsp; [All levers](../README.md) &emsp; [5 Make fewer round trips →](5-make-fewer-round-trips.md)
