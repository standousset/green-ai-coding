[All levers](../README.md) &emsp; [Lever 2: Stay in the cache window →](lever-2-stay-in-the-cache-window.md)

# Lever 1: Keep the context short

*Reading time: about 7 minutes.*

> [!IMPORTANT]
> Give the assistant only what the task needs, keep noisy output out, and compact when a long task has to keep going.

**Why it matters:** The context is everything the model reads with each request:
your messages, its replies, the tool results and the rules file. A long context
means more tokens, because every request sends the whole conversation again.

| Item | Role |
|---|---|
| [1.1 Start a new conversation for each unrelated task](#11-start-a-new-conversation-for-each-unrelated-task) | Start clean |
| [1.2 Send only what the task needs](#12-send-only-what-the-task-needs) | Prevent |
| [1.3 Keep the always-loaded context small](#13-keep-the-always-loaded-context-small) | Prevent |
| [1.4 Keep noisy output out of the context](#14-keep-noisy-output-out-of-the-context) | Prevent |
| [1.5 Go back with `/rewind` when an attempt failed](#15-go-back-with-rewind-when-an-attempt-failed) | Remove a dead end |
| [1.6 Compact when a long task must continue](#16-compact-when-a-long-task-must-continue) | Shrink, by hand |
| [1.7 Set the auto-compact window to compact earlier](#17-set-the-auto-compact-window-to-compact-earlier) | Shrink, by setting |

## 1.1 Start a new conversation for each unrelated task

**What:** Keep only the conversation the current task needs, and start a new one
when the task is unrelated.

**Why:** Each request sends the full conversation again. Each time the assistant
uses a tool, it sends one more request with the tool results ([Claude Code
docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).
Old, unrelated context makes every message cost more.

<img alt="Four horizontal bars, one for each request, each in a frame that shows the maximum context size. Each bar shows the input, then the new output. The reply of one request is part of the input of the next request, so the bars grow longer and fill more of the frame." src="../images/context-grows-each-request.svg#gh-light-mode-only">
<img alt="Four horizontal bars, one for each request, each in a frame that shows the maximum context size. Each bar shows the input, then the new output. The reply of one request is part of the input of the next request, so the bars grow longer and fill more of the frame." src="../images/context-grows-each-request-dark.svg#gh-dark-mode-only">

**How:** Use `/context` to see what fills the context. Use `/clear` to start a
new conversation between unrelated tasks.

**Where:**
- Terminal and Desktop app (Code tab): `/context` shows what fills the context,
  and `/clear` starts a new conversation. In the desktop app, type `/` in the
  prompt box to list the built-in commands
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
- Desktop app (Code tab): the **Context** card in the session details panel shows
  how full the context window is. To show the panel, click **Show session rail**
  in the title bar. To start a new session, click **+ New** or press Cmd+N
  (Ctrl+N on Windows)
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#see-session-details)).

## 1.2 Send only what the task needs

**What:** Name the file instead of making the agent search for it. Then choose what
enters the context: the file with `@`, or only the lines you name without using `@`.

**Why:** Whatever you send stays in the context and goes out again with every
request (see [1.1](#11-start-a-new-conversation-for-each-unrelated-task)).

<img alt="Two horizontal bars. With a vague request, the context holds the rules file, your message, a search and six files, and it fills most of the frame. With a precise request that names one file, the context holds the rules file, your message and one file, so it stays small." src="../images/send-only-what-the-task-needs.svg#gh-light-mode-only">
<img alt="Two horizontal bars. With a vague request, the context holds the rules file, your message, a search and six files, and it fills most of the frame. With a precise request that names one file, the context holds the rules file, your message and one file, so it stays small." src="../images/send-only-what-the-task-needs-dark.svg#gh-dark-mode-only">



**How:** Choose by what you need. Write the path of the file, not "find the
login code". Claude then doesn't need to search.

| You need | You write | What enters the context |
|---|---|---|
| A file in full | `@src/login.ts` | The content of the file, if it fits 25,000 tokens, and the `CLAUDE.md` files of its folder and parent folders |
| One part of a big file | `Read lines 40 to 80 of src/login.ts`, with no `@` | The lines that Claude reads with its Read tool |
| The structure of a folder | `@src/components/` | A list of files, not their content |
| A big log | No `@`. Ask Claude to read it, or to run a command that filters it (see [1.4](#14-keep-noisy-output-out-of-the-context)) | What the tool or the command returns |

<img alt="Two rows. In the first row, you type @ and a file name. Claude Code adds the content of the file to the context, if it fits the limit. It makes no tool call, so a PreToolUse hook does not run. In the second row, you write the file name and the lines 40 to 80 without @. Claude calls the Read tool with a line range, and those lines enter the context. A hook can run on that tool call. The sizes are illustrative, not measured." src="../images/at-sign-versus-named-lines.svg#gh-light-mode-only">
<img alt="Two rows. In the first row, you type @ and a file name. Claude Code adds the content of the file to the context, if it fits the limit. It makes no tool call, so a PreToolUse hook does not run. In the second row, you write the file name and the lines 40 to 80 without @. Claude calls the Read tool with a line range, and those lines enter the context. A hook can run on that tool call. The sizes are illustrative, not measured." src="../images/at-sign-versus-named-lines-dark.svg#gh-dark-mode-only">

Don't put `@` before the file name when you name lines. `@` adds the content of
the file, and the docs don't say that it keeps only the lines you name
([Claude Code docs](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)).
Without `@`, Claude reads a large file in pages with `offset` and `limit`
([Claude Code docs](https://code.claude.com/docs/en/tools-reference#read-tool-behavior)).

**Where:**
- Terminal and Desktop app (Code tab): type `@` in the prompt box to open a menu
  of paths. Accept the path with Enter or Tab
  ([Claude Code docs](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)).

**Watch out:**
- A text file larger than 256 KB isn't included. For a file over 25,000 tokens,
  the docs don't say what `@` adds. Name the lines instead
  ([Claude Code docs](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)).
- A `PreToolUse` hook does not run on an `@` reference, because Claude Code makes
  no tool call. So a hook cannot filter a big log that you add with `@`.

## 1.3 Keep the always-loaded context small

**What:** Keep the rules file (`CLAUDE.md`, `AGENTS.md`) short, and load the other instructions only when a
task needs them.

**Why:** Claude Code loads some content before your first prompt. This is the
system prompt, your `CLAUDE.md` files, the auto memory, the skill descriptions
and the MCP tool names. All of it goes out again with every request (see
[1.1](#11-start-a-new-conversation-for-each-unrelated-task)). The body of a skill
and a path-scoped rule load later, only when used
([Claude Code docs](https://code.claude.com/docs/en/context-window#what-the-timeline-shows)).

<img alt="A list of what Claude Code loads into the context before you type. The system prompt is the largest part and you cannot change it. The CLAUDE.md of the project, the auto memory, the skill descriptions, the CLAUDE.md in your ~/.claude folder and the MCP tool names load at start, and you can change them. The environment information is fixed. In the docs example, the total is 7,850 tokens. The body of a skill and path-scoped rules load later, only when used. The sizes are examples from the Claude Code docs, not measured here." src="../images/what-loads-before-the-first-prompt.svg#gh-light-mode-only">
<img alt="A list of what Claude Code loads into the context before you type. The system prompt is the largest part and you cannot change it. The CLAUDE.md of the project, the auto memory, the skill descriptions, the CLAUDE.md in your ~/.claude folder and the MCP tool names load at start, and you can change them. The environment information is fixed. In the docs example, the total is 7,850 tokens. The body of a skill and path-scoped rules load later, only when used. The sizes are examples from the Claude Code docs, not measured here." src="../images/what-loads-before-the-first-prompt-dark.svg#gh-dark-mode-only">

**How:**
- Run `/context` to see what loads in your session
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#check-your-own-session)).
- Keep `CLAUDE.md` or `AGENTS.md` under 200 lines. [AGENTS.md](https://agents.md/)
  is a shared format for the rules file of coding assistants
  ([Claude Code docs](https://code.claude.com/docs/en/memory#write-effective-instructions)).
- Move the instructions of one workflow, for example a pull request review, from
  `CLAUDE.md` to a skill. The body of a skill loads only when the skill is used.
  ([Claude Code docs](https://code.claude.com/docs/en/costs#move-instructions-from-claudemd-to-skills)).
- Give Claude a map of the project in a skill, for example `codebase-overview`.
  Write the key folders and the naming rules: "auth is in `src/auth/`, tests are
  `*.spec.ts`". When Claude uses the skill, it has this context at once. It
  doesn't need to read many files to learn the structure
  ([Claude Code docs](https://code.claude.com/docs/en/costs#offload-processing-to-hooks-and-skills)).
- Put the rules for one part of the project where they load only when needed.
  Use a `CLAUDE.md` or `AGENTS.md` in that folder, or a path-scoped rule (a rule
  file with `paths` in its header). Claude loads them when it reads or edits a
  matching file
  ([Claude Code docs](https://code.claude.com/docs/en/memory#how-claude-md-files-load)).
- Disable the MCP servers that you don't use: run `/mcp`. Until Claude uses a
  tool, only the tool names and the server instructions enter the context. So the
  cost is small, but it isn't zero
  ([Claude Code docs](https://code.claude.com/docs/en/costs#reduce-mcp-server-overhead)).

**Where:** Terminal and Desktop app (Code tab): both use the same `CLAUDE.md`
([Claude Code docs](https://code.claude.com/docs/en/desktop)).

**Watch out:**
- The sizes in the picture are an example from the docs. They change with the
  size of your `CLAUDE.md`, your MCP servers and your files. Your setup can add
  more, for example an output style
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#what-the-timeline-shows)).

## 1.4 Keep noisy output out of the context

**What:** Cut big tool output before it enters the context. Filter it with a
hook, or hand the work to a subagent.

**Why:** Test runs, logs and documentation searches fill the context fast. Two
ways keep this output out of your main conversation:
- A hook cuts the output before Claude sees it. For a command you know, a
  hook can keep only the useful lines, for example the last 100 lines of a test
  run ([Claude Code docs](https://code.claude.com/docs/en/costs#offload-processing-to-hooks-and-skills)).
- A subagent has its own context. It runs the noisy work there and sends back
  only a short result, so your main context stays small ([Claude Code
  docs](https://code.claude.com/docs/en/costs#delegate-verbose-operations-to-subagents)).

<img alt="Two bars for the main context and one for the subagent. Without a subagent, test logs fill the main context. With a subagent, the logs stay in the own context of the subagent, and only its short result goes back." src="../images/subagent-keeps-noise-out.svg#gh-light-mode-only">
<img alt="Two bars for the main context and one for the subagent. Without a subagent, test logs fill the main context. With a subagent, the logs stay in the own context of the subagent, and only its short result goes back." src="../images/subagent-keeps-noise-out-dark.svg#gh-dark-mode-only">

**How:**
- **Use a hook when you know the command and what its output looks like.** A
  hook is a script that Claude Code runs at a set moment. A `PreToolUse` hook
  runs before a tool runs. On `Bash`, it can change the command before it runs.
  For example:
  1. Claude wants to run `npm test`.
  2. The hook changes the command to `npm test 2>&1 | tail -n 100`.
  3. The tool runs the new command. The command prints only the last 100 lines.
  4. Only these 100 lines enter the context.

  ```text
  Claude: npm test  ->  [hook: add "| tail -n 100"]  ->  tool runs  ->  100 lines  ->  context
  ```

  The hook does not cut the output after the fact. It changes the command, so
  the long output is never made. The docs give a sample script for `npm test`,
  `pytest` and `go test`.
  ([Claude Code docs](https://code.claude.com/docs/en/costs#offload-processing-to-hooks-and-skills)).
- **Use a subagent when the work needs judgment**, such as a search in the code
  or in the docs. The subagent reads the long text in its own context and sends
  back a short answer. To write a subagent, see the
  [Claude Code docs](https://code.claude.com/docs/en/sub-agents). To run it on a
  smaller model or at a lower effort, see
  [3.4](lever-3-use-a-smaller-model.md#34-give-simple-subagent-tasks-to-haiku) and
  [4.4](lever-4-lower-the-effort.md#44-give-simple-subagent-tasks-a-low-effort).

**Where:**
- Hook, in two parts:
  1. Save the script from the docs sample as `~/.claude/hooks/filter-test-output.sh`
     ([Claude Code docs](https://code.claude.com/docs/en/costs#offload-processing-to-hooks-and-skills)).
  2. Add this block to a settings file. It runs the script before every `Bash`
     command:

     ```json
     {
       "hooks": {
         "PreToolUse": [
           {
             "matcher": "Bash",
             "hooks": [
               { "type": "command", "command": "~/.claude/hooks/filter-test-output.sh" }
             ]
           }
         ]
       }
     }
     ```

  Choose the settings file by scope: `~/.claude/settings.json` for all your
  projects (only on your machine), `.claude/settings.json` for one project (you
  can commit it), or `.claude/settings.local.json` for one project (not
  shared). To check that the hook is active, run `/hooks`: it must appear under
  `PreToolUse`
  ([Claude Code docs](https://code.claude.com/docs/en/hooks#hook-locations)).
- Subagent: in the Desktop app (Code tab), the **Background tasks** pane shows the
  subagents that run. Open it from the **⋮** menu of the title bar
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).

**Watch out:**
- A hook doesn't run on an `@` reference (see
  [1.2](#12-send-only-what-the-task-needs)). The sample in the docs covers `Bash`
  commands only.
- A subagent can't ask you questions. Claude Code removes the `AskUserQuestion`
  tool from every subagent. Permission prompts still reach you in the main
  session. Put everything the subagent needs in its task, or it has to guess
  ([Claude Code docs](https://code.claude.com/docs/en/sub-agents)).
- The requests of a subagent use energy too. A subagent doesn't use the warm
  cache of the main conversation. It starts its own cache, which lasts 5 minutes
  by default
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#which-ttl-each-request-gets)).

## 1.5 Go back with `/rewind` when an attempt failed

**What:** Take the conversation back to the message before a failed attempt. The
failed attempt drops out of the context, and you choose whether the code changes stay or
go back too.

**Why:** After a failed attempt, every next request carries the whole attempt: the
wrong replies and the tool results. `/rewind` cuts them out, and the part before
that turn is already in the cache and stays there ([Claude Code
docs](https://code.claude.com/docs/en/prompt-caching#rewinding-the-conversation)).

<img alt="Two horizontal bars. Before /rewind, the bar holds your message 1 and reply 1, then a rewind point, then the attempt that failed. The part before the rewind point stays in the cache. After /rewind, only that part remains, and the next request continues from it." src="../images/rewind-drops-dead-end.svg#gh-light-mode-only">
<img alt="Two horizontal bars. Before /rewind, the bar holds your message 1 and reply 1, then a rewind point, then the attempt that failed. The part before the rewind point stays in the cache. After /rewind, only that part remains, and the next request continues from it." src="../images/rewind-drops-dead-end-dark.svg#gh-dark-mode-only">

**How:** Type `/rewind`, then choose the message. Then choose an action
([Claude Code docs](https://code.claude.com/docs/en/checkpointing#rewind-and-summarize)).
The list below is from the terminal. The desktop app has fewer actions (see
**Where**):
- **Restore conversation** goes back and keeps the current code.
- **Restore code and conversation** goes back for both.
- **Restore code** goes back for the code and keeps the conversation.
- **Summarize from here** replaces the attempt with a short summary. Use it if
  Claude must remember what failed.
- **Summarize up to here** replaces the messages before the chosen one with a
  short summary. The later messages stay.

The two **Restore code** actions appear only when the chosen point has file
changes.

**Where:**
- Terminal: type `/rewind` in the prompt box, or press `Esc` twice when the
  prompt box is empty.
- Desktop app (Code tab): `/rewind` lets you choose the message. A window then
  opens with a **Restore** list: **Code and conversation**, **Conversation
  only** and **Code only**. The window also shows which files change. There is
  no **Summarize** action here. For that, use the terminal. The docs do not
  describe this window; we saw it in the app.

## 1.6 Compact when a long task must continue

**What:** `/compact` replaces the old conversation with a short summary.

**Why:**
- **Gain:** each request sends the whole conversation again (see 1.1).
  `/compact` replaces it with a short summary, so the context of every later request is
  smaller. Example: 100K tokens in context become 5K.

<img alt="Two horizontal bars. Before /compact, five messages and five replies fill most of the frame. After /compact, one short summary block replaces them, and the frame has room again." src="../images/compact-shrinks-context.svg#gh-light-mode-only">
<img alt="Two horizontal bars. Before /compact, five messages and five replies fill most of the frame. After /compact, one short summary block replaces them, and the frame has room again." src="../images/compact-shrinks-context-dark.svg#gh-dark-mode-only">

- **Cost, step 1:** `/compact` is one request. The model reads the whole
  conversation to write the summary. While the cache is warm, this read is
  cheap. After a break longer than the cache time, it costs the full price.
- **Cost, step 2:** the new conversation (summary and rules) is not in the cache
  yet. The `/compact` request sent the old text, not the new one. The next
  request sends it and stores it. The project context is stored again too. This
  is cheap, because the text is short
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#compacting-the-conversation)).
- **Is it worth it?** It depends on **how many requests are left** and on the
  size of the conversation. A large conversation pays off after a few requests.
  A small one needs many (see the table below).
- **Loss:** the summary is short, so it can drop details from the chat. For
  example, it can drop an instruction you gave or the exact content of a file.
  Your rules files are safe: the root `CLAUDE.md` and rules without a path are
  loaded again from disk
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#what-survives-compaction)).
  To keep a detail, name it: `/compact focus on ...` (see **How**).

**Break-even estimate.** The table shows how many more requests you need for
`/compact` to cost less than no compaction. This is our estimate, not a measure
and not a number from the docs. Cost is a proxy for tokens and energy.

Assumptions:
- The cache is warm.
- The summary has 5K tokens.
- 10K tokens of project context (`CLAUDE.md`, memory, rules) are stored again,
  even if the files did not change. The 10K is our guess: the docs give no real
  size.
- Price ratios come from the
  [pricing page](https://platform.claude.com/docs/en/about-claude/pricing).
  Cache read is 0.1x of the input price (0.05x for Sonnet 5.5 and Opus 5.5).
  Cache write is 1.25x. Output is 5x.

| Conversation before `/compact` | Break-even, cache read 0.1x | Break-even, cache read 0.05x |
|---|---|---|
| 20K | about 30 requests | about 60 requests |
| 50K | about 11 | about 21 |
| 100K | about 6 | about 10 |
| 200K | about 3 | about 5 |
| 500K | about 2 | about 3 |
| 1M | about 1.4 | about 2 |

**How:** Run `/compact` in the prompt box, in the terminal or the desktop app
([Claude Code docs](https://code.claude.com/docs/en/desktop)).
- Add instructions to choose what the summary keeps: `/compact focus on the
  auth bug fix`
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#when-your-context-fills-up)).
- Compare the number of requests left with the table above. Skip `/compact` when
  fewer requests are left than the table shows for your size. A conversation
  under 50K rarely pays off.
- In a very long session, compact when a subtask ends and more work follows. The
  docs advise natural breaks, not the middle of a task
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#compacting-the-conversation)). The docs give no
  fixed number. Each summary loses detail, so do not compact again after only a
  few requests.
- For an unrelated task, use `/clear` (see 1.1). To drop one failed attempt, see
  1.5. For the best moment, see
  [2.2 Compact before a break longer than your cache time](lever-2-stay-in-the-cache-window.md#22-compact-before-a-break-longer-than-your-cache-time).
  To compact earlier by setting, see 1.7.

## 1.7 Set the auto-compact window to compact earlier

**What:** Claude Code compacts by itself when the context is full enough. You can
set the size at which it does.

**Why:** The window depends on the model and can be very large: the docs give
about 967K tokens for Haiku 5.5. Until then, every request carries a large
context. The automatic compaction also happens when it happens, and can fall in
the middle of a task. A smaller window caps the size of each request. The
trade-off: an earlier compaction keeps less detail (see 1.6).

**How:**
- Run `/autocompact 200k` to compact earlier. Run `/autocompact auto` to return
  to the default. Run `/autocompact` alone to see the current window. The window
  can be from 100K to 1M tokens
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#set-the-auto-compact-window)).

**Where:**
- Terminal and Desktop app (Code tab): type `/autocompact 200k` in the prompt box.
- Terminal, for one launch: start Claude Code with `claude --autocompact 200k`.
- Settings file: set `"autoCompactWindow": 200000` in `~/.claude/settings.json`.

**What each way changes**
([Claude Code docs](https://code.claude.com/docs/en/model-config#set-the-auto-compact-window)):

| Way | Which models | How long | Which projects |
|---|---|---|---|
| `/autocompact 200k` | The current model only (before Claude Code 2.1.288: every model) | This session and later ones | All projects: it saves the value in your user settings |
| `claude --autocompact 200k` | Not stated | This launch only. It does not change the saved value | The project you launch in |
| `"autoCompactWindow"` in `~/.claude/settings.json` | Every model | Every session | All projects |

A value saved with `/autocompact` for a model wins over the settings key for that
model. The docs do not say if the key works in a project settings file.

---

[All levers](../README.md) &emsp; [Lever 2: Stay in the cache window →](lever-2-stay-in-the-cache-window.md)
