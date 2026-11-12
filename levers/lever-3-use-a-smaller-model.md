[← Lever 2: Stay in the cache window](lever-2-stay-in-the-cache-window.md) &emsp; [All levers](../README.md) &emsp; [Lever 4: Lower the effort →](lever-4-lower-the-effort.md)

# Lever 3: Use a smaller model

*Reading time: about 8 minutes.*

> [!IMPORTANT]
> Make a mid-size model your default, and use the largest model for the hardest tasks only.

**Why it matters:** A larger model uses more energy for each token that it writes.
Many plans start on the largest model. The Claude Code docs say that "Sonnet
handles most coding tasks well and costs less than Opus"
([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model)).
A smaller default model is the simplest saving in this guide.

In the EcoLogits method, the energy for one output token is linear in
the number of active parameters of the model. It also has a fixed part
([EcoLogits method](https://ecologits.ai/latest/methodology/llm_inference/)).
So a larger model uses more energy for each word that it writes. The method fits
models up to 200B parameters. For larger models, the page says that it
extrapolates. It is an estimate, not a measure of one given model.

A test of 26 families of language models, on software tasks, shows the same
picture. More energy did not always bring a gain in accuracy of the same size.
Three factors explained more than 90% of the energy. They are the number of
active parameters, the length of the output, and the quantization level (the
precision of the numbers that the model stores). The test used models that run on
local hardware, not Claude
([Alizadeh et al., 2024](https://arxiv.org/abs/2412.00329)).

<img alt="Three horizontal bars show the energy for one output token on a small, a mid-size and a large model. Each bar has a fixed part of the same length and a second part that grows with the number of active parameters. The large model has the longest bar." src="../images/energy-per-token-grows-with-model-size.svg#gh-light-mode-only">
<img alt="Three horizontal bars show the energy for one output token on a small, a mid-size and a large model. Each bar has a fixed part of the same length and a second part that grows with the number of active parameters. The large model has the longest bar." src="../images/energy-per-token-grows-with-model-size-dark.svg#gh-dark-mode-only">

| Item | Role |
|---|---|
| [3.1 Make Sonnet your default model, and switch for one session with `s`](#31-make-sonnet-your-default-model-and-switch-for-one-session-with-s) | Set your default |
| [3.2 Try the smaller model first and go up only when needed](#32-try-the-smaller-model-first-and-go-up-only-when-needed) | Use |
| [3.3 Plan with Opus and do the work on Sonnet](#33-plan-with-opus-and-do-the-work-on-sonnet) | Use two models |
| [3.4 Give simple subagent tasks to Haiku](#34-give-simple-subagent-tasks-to-haiku) | Use a small model for subagents |

## 3.1 Make Sonnet your default model, and switch for one session with `s`

**What:** Set Sonnet as the model that new sessions start on. To try another model
in one session, press `s` in the `/model` picker. Do not press Enter.

**Why:**
- The docs list Opus 5.5 as the default model on Pro, Max, Ultra, Team,
  Enterprise and the Anthropic API
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#default-model-setting)).
  The costs page names "Opus left as the default model" as a common cause of
  limit problems
  ([Claude Code docs](https://code.claude.com/docs/en/costs#when-a-developer-asks-about-a-limit)).
  If you never chose a model, you may use the largest one for every task. Most
  coding tasks do not need it (see 3.2). With Sonnet as your default, you use a
  smaller model unless you choose another one.
- Enter in the `/model` picker switches the model and also saves it as the default
  of all your new sessions. The `s` key switches for the current session only. If
  you try Opus once with Enter, all later sessions can start on Opus
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#setting-your-model)).

<img alt="Five rows from the strongest to the weakest way to set the model in Claude Code: /model in the session, the --model option, the ANTHROPIC_MODEL variable, the model key in a settings file, and the ANTHROPIC_DEFAULT_MODEL variable, which applies only when nothing above sets a model." src="../images/model-setting-priority.svg#gh-light-mode-only">
<img alt="Five rows from the strongest to the weakest way to set the model in Claude Code: /model in the session, the --model option, the ANTHROPIC_MODEL variable, the model key in a settings file, and the ANTHROPIC_DEFAULT_MODEL variable, which applies only when nothing above sets a model." src="../images/model-setting-priority-dark.svg#gh-dark-mode-only">

**How:**
- To set your default, add `"model": "sonnet"` to a settings file. The alias
  `sonnet` always points to the latest Sonnet model
  ([model aliases](https://code.claude.com/docs/en/model-config#model-aliases)).
- The settings files have an order. The local project file
  (`.claude/settings.local.json`) wins over the shared project file
  (`.claude/settings.json`), and that file wins over your user file
  (`~/.claude/settings.json`)
  ([settings precedence](https://code.claude.com/docs/en/settings#settings-precedence)).
- For one run, use `claude --model sonnet`. The `ANTHROPIC_MODEL` variable also
  works, and it beats the settings files. The figure above shows the order
  ([model configuration](https://code.claude.com/docs/en/model-config#setting-your-model)).

<img alt="Two rows. When you press Enter in the /model picker or type /model with a model name, the session uses the model and the choice is saved as the default in your user settings. When you press s, the session uses the model and the saved default stays unchanged." src="../images/model-switch-saved-or-session-only.svg#gh-light-mode-only">
<img alt="Two rows. When you press Enter in the /model picker or type /model with a model name, the session uses the model and the choice is saved as the default in your user settings. When you press s, the session uses the model and the saved default stays unchanged." src="../images/model-switch-saved-or-session-only-dark.svg#gh-dark-mode-only">

To switch for one session only, use the picker:
1. Type `/model` with no model name. Typing `/model opus` behaves like Enter and
   saves the choice.
2. Move to the model with the arrow keys.
3. Press `s`.

You can also use `/model` in a session started with `-p`. This applies to the
current session only
([Claude Code docs](https://code.claude.com/docs/en/model-config#a-new-session-starts-on-a-different-model-than-you-picked)).

**Where:**
- Terminal: the settings and the option above. `/model` shows the model in use.
  `/usage` shows a **Usage by model** list with the tokens of each model
  ([Claude Code docs](https://code.claude.com/docs/en/costs#using-the-usage-command)).
  The `/model` command needs Claude Code 2.1.205 or later in `-p` mode.
- Desktop app, Code tab: choose the model with the picker in the row under the
  prompt box. Cmd+Shift+I opens the model menu
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#start-a-session)).
  The docs do not say whether the picker saves your choice for new sessions, and
  they do not describe the `s` key. A help center article says that `/model` works
  in Claude Desktop
  ([Claude help center](https://support.claude.com/en/articles/11940350-claude-code-model-configuration)).

**Watch out:** A `model` key in a project settings file wins over your saved
default. Your next session can start on the project model.

## 3.2 Try the smaller model first and go up only when needed

**What:** Start each type of task on Sonnet, or on Haiku if the task is simple. Go
up one step only when the result is not good enough.

**Why:** The docs reserve Opus "for complex architectural decisions or multi-step
reasoning"
([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model)).
The `haiku` alias is for simple tasks, and `sonnet` is for daily coding tasks
([model aliases](https://code.claude.com/docs/en/model-config#model-aliases)).

<img alt="Three steps. Haiku is for simple tasks. Sonnet is for most coding tasks and is the place to start. Opus is for architecture decisions and multi-step reasoning. An arrow that points up says to go up one step only if the result is not good enough." src="../images/start-small-go-up-when-needed.svg#gh-light-mode-only">
<img alt="Three steps. Haiku is for simple tasks. Sonnet is for most coding tasks and is the place to start. Opus is for architecture decisions and multi-step reasoning. An arrow that points up says to go up one step only if the result is not good enough." src="../images/start-small-go-up-when-needed-dark.svg#gh-dark-mode-only">

**How:** Start on Sonnet. Use Haiku for a simple, clear task. If the result is
not good enough, repeat the task on the next model. Choose the model before the
first message of a task. A switch in the middle reads the whole conversation
again, with no cache (see
[2.4](lever-2-stay-in-the-cache-window.md#24-do-not-change-the-model-in-the-middle-of-a-task)).

**Where:**
- Terminal: the `/model` picker. Press `s` to keep your saved default (see 3.1).
- Desktop app, Code tab: the model picker. You can change it during the session
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#start-a-session)).

**Watch out:**
- The docs describe the aliases, not a test for your own tasks. Check the result on
  your own code before you trust the smaller model.
- A smaller model that needs several corrections can cost more than a larger model
  that gets it right the first time. Each correction sends the whole conversation
  again (see [lever 5](lever-5-make-fewer-round-trips.md)). If the smaller model
  fails twice on a task, start the task again on the next model (see
  [5.5](lever-5-make-fewer-round-trips.md#55-after-two-failed-corrections-run-clear-and-write-a-better-prompt)).
  No measure exists of where the break-even is.

## 3.3 Plan with Opus and do the work on Sonnet

**What:** For a task that needs a plan, plan with Opus and do the work on
Sonnet. If you can, clear the planning context before the work starts.

**Why:**
- The plan needs the most reasoning of the task. The docs reserve Opus "for
  complex architectural decisions or multi-step reasoning"
  ([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model)).
  The work that follows is mostly code, and Sonnet handles most coding tasks.
- `opusplan` does this for you. It uses Opus in plan mode and Sonnet in execution
  mode
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#opusplan-model-setting)).
- Each model has its own cache. So each switch between plan mode and execution
  mode starts a fresh cache
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#switching-models)).
  One switch is fine. Many switches are not.
- Planning fills the context with the files that Claude read to explore. If the
  work starts from the plan alone, each request of the work is smaller (see
  [lever 1](lever-1-keep-the-context-short.md)).

<img alt="Two timelines. With opusplan, plan mode runs on Opus and work mode runs on Sonnet, and each model has its own cache. With one plan and a long work phase, there is one switch. With six toggles between plan mode and work mode, there are six fresh caches." src="../images/opusplan-switches-cache.svg#gh-light-mode-only">
<img alt="Two timelines. With opusplan, plan mode runs on Opus and work mode runs on Sonnet, and each model has its own cache. With one plan and a long work phase, there is one switch. With six toggles between plan mode and work mode, there are six fresh caches." src="../images/opusplan-switches-cache-dark.svg#gh-dark-mode-only">

**How:** There are three ways. Choose one.
1. **One session.** Run `/model opusplan`. Press Shift+Tab to enter plan mode, or
   start a prompt with `/plan`. When you approve the plan, the work runs on
   Sonnet in the same conversation. It keeps the planning conversation, which
   the new model reads once with no cache. Do not toggle plan mode many times
   ([Claude Code docs](https://code.claude.com/docs/en/permission-modes#analyze-before-you-edit-with-plan-mode)).
2. **One session, with a clean start.** Do the same, and set
   `showClearContextOnPlanAccept` to `true` in a settings file. The approval menu
   then gets a first option, **Yes, clear context and …**. It clears the
   conversation and starts the work from the plan alone
   ([Claude Code docs](https://code.claude.com/docs/en/settings-reference#showclearcontextonplanaccept)).
   The docs describe this setting and `opusplan` separately. They do not say what
   the two do together.
3. **Two sessions.** Plan in a session on Opus. Claude Code saves the plan as a
   file in `~/.claude/plans`, or in the folder of the `plansDirectory` setting
   ([Claude Code docs](https://code.claude.com/docs/en/settings-reference#plansdirectory)).
   Then start a new session on Sonnet and ask Claude to do the plan in that file.

Way 3 needs no special setting and works in every app. Ways 2 and 3 give the
smallest context for the work. Choose way 1 if the work needs what Claude learned
while it planned.

**Where:** In the terminal, use any of the three ways. In the desktop app, Code tab,
Plan is a permission mode
([Claude Code docs](https://code.claude.com/docs/en/desktop)). The docs do not
mention `opusplan` or the clear-context option there. Way 3 works: choose Opus in
the model picker for the planning session, and Sonnet for the work session.

**Watch out:** If the context is cleared, the plan is all that Claude keeps. Read
the plan before you approve it. Press `Ctrl+G` to open it in your editor
([Claude Code docs](https://code.claude.com/docs/en/permission-modes#review-and-approve-a-plan)).
For the reasons to plan at all, and when a plan is not worth it, see
[5.2](lever-5-make-fewer-round-trips.md#52-agree-on-a-plan-before-claude-edits-anything).

## 3.4 Give simple subagent tasks to Haiku

**What:** For a subagent that does a simple task, set `model: haiku` in its file.

**Why:** A subagent with no model of its own uses the model of the main
conversation, unless a variable sets another one. A switch to Opus
also applies to the subagents that inherit your model
([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model)).
The docs advise `model: haiku` "for simple subagent tasks". The built-in Explore
subagent also uses the model of the main conversation, so it runs on Opus when
you work on Opus
([Claude Code docs](https://code.claude.com/docs/en/sub-agents#built-in-subagents)).

<img alt="Two horizontal bars. The main conversation runs on Sonnet and sends a task to a subagent that runs on Haiku. The subagent reads a search result and three files, which is noisy tool output, and writes a short summary. Only the summary returns to the main conversation." src="../images/subagent-on-haiku.svg#gh-light-mode-only">
<img alt="Two horizontal bars. The main conversation runs on Sonnet and sends a task to a subagent that runs on Haiku. The subagent reads a search result and three files, which is noisy tool output, and writes a short summary. Only the summary returns to the main conversation." src="../images/subagent-on-haiku-dark.svg#gh-dark-mode-only">

**How:**
- Add `model: haiku` to the front matter of the subagent file.
- The docs give an order, from the strongest to the weakest
  ([Claude Code docs](https://code.claude.com/docs/en/sub-agents#choose-a-model)):
  1. The model that Claude passes for one call.
  2. The `model` key of the subagent file.
  3. The `CLAUDE_CODE_SUBAGENT_MODEL` variable.
  4. The model of the main conversation.
- To run Explore on a small model, define your own subagent named Explore with
  `model: haiku`. It replaces the built-in one.
- To run every subagent on one model, set `CLAUDE_CODE_SUBAGENT_MODEL` to `haiku`
  and `CLAUDE_CODE_SUBAGENT_MODEL_FORCE` to `1` (Claude Code 2.1.257 or later).

**Where:**
- Terminal: `/tasks` shows the model of each subagent (Claude Code 2.1.242 or
  later).
- Desktop app, Code tab: the Background tasks pane lists subagents
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)). The docs do not
  describe the model of a subagent there.

**Watch out:** Without the `FORCE` variable, `CLAUDE_CODE_SUBAGENT_MODEL` is only
a default. A `model` key in a subagent file still wins over it.

---

[← Lever 2: Stay in the cache window](lever-2-stay-in-the-cache-window.md) &emsp; [All levers](../README.md) &emsp; [Lever 4: Lower the effort →](lever-4-lower-the-effort.md)
