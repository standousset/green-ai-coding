[← Lever 2: Stay in the cache window](lever-2-stay-in-the-cache-window.md) &emsp; [All levers](../README.md) &emsp; [Lever 4: Lower the effort →](lever-4-lower-the-effort.md)

# Lever 3: Use a smaller model

> [!IMPORTANT]
> Make a mid-size model your default, and use the largest model for the hardest tasks only.

**Why it matters:** A larger model uses more energy for each token that it writes
(see 3.1). Many plans start on the largest model. The Claude Code docs say that
"Sonnet handles most coding tasks well and costs less than Opus"
([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model)).
A smaller default model is the simplest saving in this guide.

| Item | Role |
|---|---|
| [3.1 Know that a larger model uses more energy for each token](#31-know-that-a-larger-model-uses-more-energy-for-each-token) | Learn the rule |
| [3.2 Check which model you use now](#32-check-which-model-you-use-now) | Learn your model |
| [3.3 Make Sonnet your default model](#33-make-sonnet-your-default-model) | Set your default |
| [3.4 Try the smaller model first and go up only when needed](#34-try-the-smaller-model-first-and-go-up-only-when-needed) | Use |
| [3.5 Press `s` in the `/model` picker to switch for one session only](#35-press-s-in-the-model-picker-to-switch-for-one-session-only) | Protect your default |
| [3.6 Use `opusplan` for one plan followed by a long work phase](#36-use-opusplan-for-one-plan-followed-by-a-long-work-phase) | Use two models |
| [3.7 Give simple subagent tasks to Haiku](#37-give-simple-subagent-tasks-to-haiku) | Use a small model for subagents |

## 3.1 Know that a larger model uses more energy for each token

**What:** Treat the size of the model as a cost. Use the smallest model that does
the task well.

**Why:** In the EcoLogits method, the energy for one output token is linear in
the number of active parameters of the model. It also has a fixed part
([EcoLogits method](https://ecologits.ai/latest/methodology/llm_inference/)).
So a larger model uses more energy for each word that it writes. The method fits
models up to 200B parameters. For larger models, the page says that it
extrapolates. It is an estimate, not a measure of one given model.

![Three horizontal bars show the energy for one output token on a small, a mid-size and a large model. Each bar has a fixed part of the same length and a second part that grows with the number of active parameters. The large model has the longest bar.](../images/energy-per-token-grows-with-model-size.svg)

**How:** Choose the model with the items below. For the effect of the model on
the cache, see [2.4](lever-2-stay-in-the-cache-window.md#24-do-not-change-the-model-in-the-middle-of-a-task).

**Where:** This applies to all three ways to use Claude.

## 3.2 Check which model you use now

**What:** Find out which model your sessions start on.

**Why:** The docs list Opus 5.5 as the default model on Pro, Max, Ultra, Team,
Enterprise and the Anthropic API
([Claude Code docs](https://code.claude.com/docs/en/model-config#default-model-setting)).
The costs page names "Opus left as the default model" as a common cause of
limit problems
([Claude Code docs](https://code.claude.com/docs/en/costs#when-a-developer-asks-about-a-limit)).
If you never chose a model, you may use the largest one for every task.

**How:** Look at the picker or at the usage by model, as in the list below.

**Where:**
- Terminal: `/model` opens the picker and shows the model in use. `/usage` shows
  a **Usage by model** list with the tokens of each model
  ([Claude Code docs](https://code.claude.com/docs/en/costs#using-the-usage-command)).
- Desktop app, Code tab: the model picker is in the row under the prompt box
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#start-a-session)).
  Cmd+Shift+I opens the model menu
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#keyboard-shortcuts)).
- Chat: the model name is next to the send button. Click it to see the other
  models ([Claude help center](https://support.claude.com/en/articles/8664678-change-the-model-effort-and-thinking-settings)).

## 3.3 Make Sonnet your default model

**What:** Set Sonnet as the model that new sessions start on.

**Why:** Most coding tasks do not need the largest model (see 3.4). If your
default is Sonnet, you use a smaller model unless you choose another one.

![Five rows from the strongest to the weakest way to set the model in Claude Code: /model in the session, the --model option, the ANTHROPIC_MODEL variable, the model key in a settings file, and the ANTHROPIC_DEFAULT_MODEL variable, which applies only when nothing above sets a model.](../images/model-setting-priority.svg)

**How:** Add `"model": "sonnet"` to a settings file. The alias `sonnet` always
points to the latest Sonnet model
([model aliases](https://code.claude.com/docs/en/model-config#model-aliases)).
The settings files have an order. The local project file
(`.claude/settings.local.json`) wins over the shared project file
(`.claude/settings.json`), and that file wins over your user file
(`~/.claude/settings.json`)
([settings precedence](https://code.claude.com/docs/en/settings#settings-precedence)).
If a project file sets a `model`, new sessions in that project start on it,
whatever your user file says. For one run, use `claude --model sonnet`. The
`ANTHROPIC_MODEL` variable also works, and it beats the settings files. The
order is in the figure above
([model configuration](https://code.claude.com/docs/en/model-config#setting-your-model)).

**Where:**
- Terminal: the settings and the option above.
- Desktop app, Code tab: choose the model with the picker under the prompt box.
  The docs do not say whether the picker saves your choice for new sessions.
- Chat: choose the model with the model menu next to the send button. A change
  applies from the next reply of Claude
  ([Claude help center](https://support.claude.com/en/articles/8664678-change-the-model-effort-and-thinking-settings)).
  The model you choose has an effect on how fast you reach the usage limit
  ([Claude help center](https://support.claude.com/en/articles/11647753-how-do-usage-and-length-limits-work)).
  The help center gives no advice on the size of the model.

## 3.4 Try the smaller model first and go up only when needed

**What:** Start each type of task on the smaller model. Go up one step only when
the result is not good enough.

**Why:** The docs reserve Opus "for complex architectural decisions or multi-step
reasoning"
([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model)).
The `haiku` alias is for simple tasks, and `sonnet` is for daily coding tasks
([model aliases](https://code.claude.com/docs/en/model-config#model-aliases)).

![Three steps. Haiku is for simple tasks. Sonnet is for most coding tasks and is the place to start. Opus is for architecture decisions and multi-step reasoning. An arrow that points up says to go up one step only if the result is not good enough.](../images/start-small-go-up-when-needed.svg)

**How:** Start on Sonnet. Use Haiku for a simple, clear task. If the result is
not good enough, repeat the task on the next model. Choose the model before the
first message of a task. A switch in the middle reads the whole conversation
again, with no cache (see
[2.4](lever-2-stay-in-the-cache-window.md#24-do-not-change-the-model-in-the-middle-of-a-task)).

**Where:**
- Terminal: `/model sonnet`, `/model haiku` or `/model opus`.
- Desktop app, Code tab: the model picker. You can change it during the session
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#start-a-session)).
- Chat: the model menu next to the send button.

**Watch out:** The docs describe the aliases, not a test for your own tasks. Check
the result on your own code before you trust the smaller model.

## 3.5 Press `s` in the `/model` picker to switch for one session only

**What:** When you open `/model` and choose a model, press `s`. Do not press Enter.

**Why:** In the picker, Enter switches the model and saves it as your default for
new sessions. The `s` key switches for this session only and leaves your default
unchanged. Typing `/model` with a model name behaves like Enter
([Claude Code docs](https://code.claude.com/docs/en/model-config#setting-your-model)).
If you try Opus once with Enter, all later sessions can start on Opus.

![Two rows. When you press Enter in the /model picker or type /model with a model name, the session uses the model and the choice is saved as the default in your user settings. When you press s, the session uses the model and the saved default stays unchanged.](../images/model-switch-saved-or-session-only.svg)

**How:** Open `/model`, move to the model, and press `s`. In a session started
with `-p`, the choice applies to the current session only and is not saved
([Claude Code docs](https://code.claude.com/docs/en/model-config#setting-your-model)).

**Where:**
- Terminal: the keys above. The `/model` command needs Claude Code 2.1.205 or
  later in `-p` mode.
- Desktop app, Code tab: the docs do not describe the `s` key. A help center
  article says that `/model` works in Claude Desktop
  ([Claude help center](https://support.claude.com/en/articles/11940350-claude-code-model-configuration)).

**Watch out:** A `model` key in a project settings file still wins over your saved
default (see 3.3). Your next session can start on the project model.

## 3.6 Use `opusplan` for one plan followed by a long work phase

**What:** Set the model to `opusplan` when you plan once and then work for a long
time.

**Why:** `opusplan` uses Opus in plan mode and Sonnet in execution mode
([Claude Code docs](https://code.claude.com/docs/en/model-config#opusplan-model-setting)).
Each model has its own cache. Each switch between plan mode and execution mode
starts a fresh cache
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).
With one plan and a long work phase, you pay for one switch, and Sonnet does the
work phase.

![Two timelines. With opusplan, plan mode runs on Opus and work mode runs on Sonnet, and each model has its own cache. With one plan and a long work phase, there is one switch. With six toggles between plan mode and work mode, there are six fresh caches.](../images/opusplan-switches-cache.svg)

**How:** Run `/model opusplan`. Make the plan, then leave plan mode and work. Do
not toggle plan mode many times in one task. For the cost of a switch, see
[2.4](lever-2-stay-in-the-cache-window.md#24-do-not-change-the-model-in-the-middle-of-a-task).

**Where:**
- Terminal: the steps above.
- Desktop app, Code tab: Plan is a permission mode there
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)). The docs do not
  mention `opusplan` for the desktop app.

## 3.7 Give simple subagent tasks to Haiku

**What:** For a subagent that does a simple task, set `model: haiku` in its file.

**Why:** A subagent with no model of its own uses the model of the main
conversation, unless a variable sets another one. A switch to Opus
also applies to the subagents that inherit your model
([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model)).
The docs advise `model: haiku` "for simple subagent tasks". The built-in Explore
subagent also uses the model of the main conversation, so it runs on Opus when
you work on Opus
([Claude Code docs](https://code.claude.com/docs/en/sub-agents#built-in-subagents)).

![Two horizontal bars. The main conversation runs on Sonnet and sends a task to a subagent that runs on Haiku. The subagent reads a search result and three files, which is noisy tool output, and writes a short summary. Only the summary returns to the main conversation.](../images/subagent-on-haiku.svg)

**How:** Add `model: haiku` to the front matter of the subagent file. The order
of the docs has four steps. First, the model that Claude passes for one call.
Then the `model` key of the subagent file, then the
`CLAUDE_CODE_SUBAGENT_MODEL` variable, and last the model of the main
conversation ([Claude Code docs](https://code.claude.com/docs/en/sub-agents#choose-a-model)).
To run Explore on a small model, define your own subagent named Explore with
`model: haiku`: it replaces the built-in one. To run every subagent on one
model, set `CLAUDE_CODE_SUBAGENT_MODEL` to `haiku` and
`CLAUDE_CODE_SUBAGENT_MODEL_FORCE` to `1` (Claude Code 2.1.257 or later).

**Where:**
- Terminal: the steps above. `/tasks` shows the model of each subagent (Claude
  Code 2.1.242 or later).
- Desktop app, Code tab: the Background tasks pane lists subagents
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)). The docs do not
  describe the model of a subagent there.

**Watch out:** Without the `FORCE` variable, `CLAUDE_CODE_SUBAGENT_MODEL` is only
a default. A `model` key in a subagent file still wins over it.

---

[← Lever 2: Stay in the cache window](lever-2-stay-in-the-cache-window.md) &emsp; [All levers](../README.md) &emsp; [Lever 4: Lower the effort →](lever-4-lower-the-effort.md)
