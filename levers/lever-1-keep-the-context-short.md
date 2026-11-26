[All levers](../README.md) &emsp; [Lever 2: Stay in the cache window →](lever-2-stay-in-the-cache-window.md)

# Lever 1: Keep the context short


## Give the assistant only what the task needs, keep noisy output out, and compact when a long task has to keep going.

*Reading time: about 7 minutes.*


**Why it matters:** The context is everything the model reads with each request:
your messages, its replies, the tool results and the rules file. A long context
means a more tokens, because every request sends the whole conversation again.

| Item | Role |
|---|---|
| [1.1 Start a new conversation for each unrelated task](#11-start-a-new-conversation-for-each-unrelated-task) | Start clean |
| [1.2 Send only what the task needs](#12-send-only-what-the-task-needs) | Prevent |
| [1.3 Keep the always-loaded context small](#13-keep-the-always-loaded-context-small) | Prevent |
| [1.4 Keep noisy output out of the context](#14-keep-noisy-output-out-of-the-context) | Prevent |
| [1.5 Go back with `/rewind` when an attempt failed](#15-go-back-with-rewind-when-an-attempt-failed) | Remove a dead end |
| [1.6 Compact when a long task must continue](#16-compact-when-a-long-task-must-continue) | Shrink, by hand or by setting |

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

**Related tasks:** The choice is not only about relatedness. A related task can
still need a new conversation, depending on the size of the context and the
cache:
- **Keep the conversation** when the next task needs the context and the context
  is small. The old context is in the cache, and a cache read costs 0.1 times the
  input price on most models (0.05 times on Opus 5.5 and Sonnet 5.5). Check that
  the cache is still warm first ([2.1](lever-2-stay-in-the-cache-window.md#21-check-how-long-your-cache-lasts)).
- **Clear the conversation** when the first task filled a lot of context. Every
  request still reads all of that context, even from the cache. The handoff note
  also costs something: writing it into a new cache costs 1.25 times the input
  price. Compare the two costs before you decide.


**Where:**
- Terminal and desktop app, Code tab: `/context` shows what fills the context,
  and `/clear` starts a new conversation. In the desktop app, type `/` in the
  prompt box to list the built-in commands
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
- Desktop app, Code tab: the **Context** card in the session details panel shows
  how full the context window is. To show the panel, click **Show session rail**
  in the title bar. To start a new session, click **+ New** or press Cmd+N
  (Ctrl+N on Windows)
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#see-session-details)).

## 1.2 Send only what the task needs

**What:** Name the file instead of making Claude search for it. Then choose what
enters the context: the file with `@`, or only the lines you name.

**Why:** Whatever you send stays in the context and goes out again with every
request (see [1.1](#11-start-a-new-conversation-for-each-unrelated-task)).

<img alt="Two horizontal bars. With a vague request, the context holds the rules file, your message, a search and six files, and it fills most of the frame. With a precise request that names one file, the context holds the rules file, your message and one file, so it stays small." src="../images/send-only-what-the-task-needs.svg#gh-light-mode-only">
<img alt="Two horizontal bars. With a vague request, the context holds the rules file, your message, a search and six files, and it fills most of the frame. With a precise request that names one file, the context holds the rules file, your message and one file, so it stays small." src="../images/send-only-what-the-task-needs-dark.svg#gh-dark-mode-only">

<img alt="Two rows. In the first row, you type @ and a file name. Claude Code adds the content of the file to the context, if it fits the limit. It makes no tool call, so a PreToolUse hook does not run. In the second row, you write the file name and the lines 40 to 80 without @. Claude calls the Read tool with a line range, and those lines enter the context. A hook can run on that tool call. The sizes are illustrative, not measured." src="../images/at-sign-versus-named-lines.svg#gh-light-mode-only">
<img alt="Two rows. In the first row, you type @ and a file name. Claude Code adds the content of the file to the context, if it fits the limit. It makes no tool call, so a PreToolUse hook does not run. In the second row, you write the file name and the lines 40 to 80 without @. Claude calls the Read tool with a line range, and those lines enter the context. A hook can run on that tool call. The sizes are illustrative, not measured." src="../images/at-sign-versus-named-lines-dark.svg#gh-dark-mode-only">

**How:** Choose by what you need. Write the path of the file, not "find the
login code". Claude then doesn't need to search.

| You need | You write | What enters the context |
|---|---|---|
| A file in full | `@src/login.ts` | The content of the file, if it fits 25,000 tokens, and the `CLAUDE.md` files of its folder and parent folders |
| One part of a big file | `Read lines 40 to 80 of src/login.ts`, with no `@` | The lines that Claude reads with its Read tool |
| The structure of a folder | `@src/components/` | A list of files, not their content |
| A big log | No `@`. Ask Claude to read it, or to run a command that filters it (see [1.4](#14-keep-noisy-output-out-of-the-context)) | What the tool or the command returns |

Don't put `@` before the file name when you name lines. `@` adds the content of
the file, and the docs don't say that it keeps only the lines you name
([Claude Code docs](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)).
Without `@`, Claude reads a large file in pages with `offset` and `limit`
([Claude Code docs](https://code.claude.com/docs/en/tools-reference#read-tool-behavior)).

**Where:**
- Terminal and desktop app, Code tab: type `@` in the prompt box to open a menu
  of paths. Accept the path with Enter or Tab
  ([Claude Code docs](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)).
- VS Code and JetBrains extensions: select lines in the editor. The extension
  shares the selection with Claude. The JetBrains shortcut inserts a reference
  with the line range, for example `@src/auth.ts#L1-99`. The docs don't say if
  only these lines or the whole file enter the context
  ([JetBrains docs](https://code.claude.com/docs/en/jetbrains),
  [VS Code docs](https://code.claude.com/docs/en/vs-code)).

**Watch out:**
- A text file larger than 256 KB isn't included. For a file over 25,000 tokens,
  the docs don't say what `@` adds. Name the lines instead
  ([Claude Code docs](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)).
- A `PreToolUse` hook does not run on an `@` reference, because Claude Code makes
  no tool call. So a hook cannot filter a big log that you add with `@`.
- To stop `@` from including content, set `CLAUDE_CODE_DISABLE_FILE_MENTIONS`
  to `1`. Use your shell, or the `env` block of your user or managed settings.
  This needs Claude Code 2.1.299 or later. Claude then gets only the path. Your
  hooks run when it opens the file. This changes the table above: `@` no longer adds
  content. A project or local settings file ignores this variable
  ([Claude Code docs](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)).
- To block one path, add a `Read` deny rule. Claude Code makes a best-effort
  attempt to apply it to `@` mentions too
  ([Claude Code docs](https://code.claude.com/docs/en/permissions#read-and-edit)).

## 1.3 Keep the always-loaded context small

**What:** Keep the rules file short, and load the other instructions only when a
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
  A skill is also a way to repeat a workflow: you write it once, and you can call
  it by name
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

**Where:** Terminal and desktop app, Code tab: both use the same `CLAUDE.md`
([Claude Code docs](https://code.claude.com/docs/en/desktop)).

**Watch out:**
- The sizes in the picture are an example from the docs. They change with the
  size of your `CLAUDE.md`, your MCP servers and your files. Your setup can add
  more, for example an output style
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#what-the-timeline-shows)).
- The description of a skill loads at start. Only its body waits until the skill
  is used. The text of a skill uses tokens when Claude loads it. Write only what is costly
  for Claude to find alone. A long map can cost more than the reading it saves.
- A skill doesn't make `@` cheaper. A large file that you add with `@` costs the
  same tokens with or without the skill (see
  [1.2](#12-send-only-what-the-task-needs)).
- After a compaction, Claude loads a path-scoped rule or a `CLAUDE.md` of a
  folder again only when it touches a matching file. If a rule must always
  apply, keep it in the `CLAUDE.md` at the project root
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#what-survives-compaction)).

## 1.4 Keep noisy output out of the context

**What:** Cut big tool output before it enters the context. Filter it with a
hook, or hand the work to a subagent. A subagent is a helper agent with its own
context. It sends back only a short result.

**Why:** Test runs, logs and documentation searches fill the context fast. Run
them in a subagent and they stay out of your main conversation ([Claude Code
docs](https://code.claude.com/docs/en/costs#delegate-verbose-operations-to-subagents)).

<img alt="Two bars for the main context and one for the subagent. Without a subagent, test logs fill the main context. With a subagent, the logs stay in the own context of the subagent, and only its short result goes back." src="../images/subagent-keeps-noise-out.svg#gh-light-mode-only">
<img alt="Two bars for the main context and one for the subagent. Without a subagent, test logs fill the main context. With a subagent, the logs stay in the own context of the subagent, and only its short result goes back." src="../images/subagent-keeps-noise-out-dark.svg#gh-dark-mode-only">

**How:**
- Use a hook when you know the command and its output. A `PreToolUse` hook on
  `Bash` can rewrite the command, for example to `npm test 2>&1 | tail -n 100`.
  Claude then sees only the last 100 lines. The hook runs before the tool. It
  changes the command, so that the tool makes less output. That output is what
  enters the context.

  ```text
  command  ->  [hook rewrites it]  ->  tool runs  ->  short output  ->  context
  ```

  The docs give a sample script for `npm test`, `pytest` and `go test`. To check
  it, run `/hooks`
  ([Claude Code docs](https://code.claude.com/docs/en/costs#offload-processing-to-hooks-and-skills)).
- Use a subagent when the work needs judgment, such as a search in the code or in
  the docs. To write a subagent, see the
[Claude Code docs](https://code.claude.com/docs/en/sub-agents). To run it on a
smaller model or at a lower effort, see
[3.4](lever-3-use-a-smaller-model.md#34-give-simple-subagent-tasks-to-haiku) and
[4.4](lever-4-lower-the-effort.md#44-give-simple-subagent-tasks-a-low-effort).

**Where:**
- Hook: add it to your `settings.json`
  ([Claude Code docs](https://code.claude.com/docs/en/costs#offload-processing-to-hooks-and-skills)).
- Subagent: in the desktop app, Code tab, the **Background tasks** pane shows the
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
attempt drops out of the context, and you choose whether the code changes stay or
go back too.

**Why:** After a failed attempt, every next request carries the whole attempt: the
wrong replies and the tool results. `/rewind` cuts them out, and the part before
that turn is already in the cache and stays there ([Claude Code
docs](https://code.claude.com/docs/en/prompt-caching#rewinding-the-conversation)).

<img alt="Two horizontal bars. Before /rewind, the bar holds your message 1 and reply 1, then a rewind point, then the attempt that failed. The part before the rewind point stays in the cache. After /rewind, only that part remains, and the next request continues from it." src="../images/rewind-drops-dead-end.svg#gh-light-mode-only">
<img alt="Two horizontal bars. Before /rewind, the bar holds your message 1 and reply 1, then a rewind point, then the attempt that failed. The part before the rewind point stays in the cache. After /rewind, only that part remains, and the next request continues from it." src="../images/rewind-drops-dead-end-dark.svg#gh-dark-mode-only">

**How:** Type `/rewind`, then choose the message. Then choose an action
([Claude Code docs](https://code.claude.com/docs/en/checkpointing#rewind-and-summarize)):
- **Restore conversation** goes back and keeps the current code.
- **Restore code and conversation** goes back for both.
- **Summarize from here** replaces the attempt with a short summary. Use it if
  Claude must remember what failed.

**Where:**
- Terminal and desktop app, Code tab: type `/rewind` in the prompt box. In the
  terminal, you can also press `Esc` twice when the prompt box is empty.

## 1.6 Compact when a long task must continue

**What:** `/compact` replaces the old conversation with a short summary. You can
also choose when Claude Code compacts by itself.

**Why:**
- In a long task, each request is large, because it sends the whole conversation
  again (see 1.1). `/compact` replaces the old messages with a summary. Each
  later request sends the summary and the new messages only.
- Example: 100K tokens of conversation become a summary of 5K tokens. The next
  requests send about 5K tokens plus the new messages, and not 100K. The saving
  repeats on every request, so it grows as the task continues. The numbers are an
  example, not a measure.
- The summary itself costs one request: the model reads the full conversation once
  more and writes the summary. Compacting a large context is a large request in
  itself ([Claude Code
  docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).
- The summary keeps less than the full conversation. The `CLAUDE.md` at the
  project root is read from disk again, so it isn't lost
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#what-survives-compaction)).

<img alt="Two horizontal bars. Before /compact, five messages and five replies fill most of the frame. After /compact, one short summary block replaces them, and the frame has room again." src="../images/compact-shrinks-context.svg#gh-light-mode-only">
<img alt="Two horizontal bars. Before /compact, five messages and five replies fill most of the frame. After /compact, one short summary block replaces them, and the frame has room again." src="../images/compact-shrinks-context-dark.svg#gh-dark-mode-only">

**How:** When a long task must continue, run `/compact` instead of carrying the
full conversation.
- To choose what the summary keeps, add instructions: `/compact focus on the
  auth bug fix`. The summary then keeps what you choose, and not what the
  automatic pass guesses is important
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#when-your-context-fills-up)).
- When the next task is unrelated, use `/clear` (see 1.1). It costs nothing.
- To drop one failed attempt and keep the rest, see 1.5.
- For the best moment to compact, see
  [2.2 Compact before a break longer than your cache time](lever-2-stay-in-the-cache-window.md#22-compact-before-a-break-longer-than-your-cache-time).
- Claude Code also compacts by itself when the context is full enough. The window
  is the context size at which this happens. It depends on the model and can be
  very large: the docs give about 967K tokens for Haiku 5.5. You don't choose the
  moment, and it can happen in the middle of a task.
- To compact earlier, run `/autocompact 200k`. Run `/autocompact auto` to return
  to the default. Run `/autocompact` alone to see the current window. The window
  can be from 100K to 1M tokens
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#set-the-auto-compact-window)).

**Where:**
- Terminal and desktop app, Code tab: type `/compact`, with or without
  instructions, in the prompt box
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
- Terminal and desktop app, Code tab: type `/autocompact` in the prompt box.
  `/autocompact` saves the value for the current model. Before Claude Code
  2.1.288, it saved one window for every model.
- Terminal, for one launch: start Claude Code with `claude --autocompact 200k`.
  The option doesn't change your saved setting.
- Settings file, for every model: set `"autoCompactWindow": 200000` in
  `~/.claude/settings.json`. A value that you saved with `/autocompact` for a
  model wins over this key for that model
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#set-the-auto-compact-window)).

---

[All levers](../README.md) &emsp; [Lever 2: Stay in the cache window →](lever-2-stay-in-the-cache-window.md)
