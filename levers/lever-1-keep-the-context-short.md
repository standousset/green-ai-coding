[All levers](../README.md) &emsp; [Lever 2: Stay in the cache window →](lever-2-stay-in-the-cache-window.md)

# Lever 1: Keep the context short

*Reading time: about 7 minutes.*

> [!IMPORTANT]
> Give the assistant only what the task needs, hand the noisy work to a subagent, and compact when a long task has to keep going.

**Why it matters:** The context is everything the model reads with each request:
your messages, its replies, the tool results and the rules file. A long context
means a lot more tokens, because every request sends the whole conversation again
(see 1.1). It also makes the answers less reliable. In a study of 18 models, the
results became less reliable as the input got longer ([Chroma, Context
Rot](https://www.trychroma.com/research/context-rot)). And less reliable answers
mean more retries (see [lever 5](lever-5-make-fewer-round-trips.md)).

| Item | Role |
|---|---|
| [1.1 Start a new conversation for each unrelated task](#11-start-a-new-conversation-for-each-unrelated-task) | Start clean |
| [1.2 Send only what the task needs](#12-send-only-what-the-task-needs) | Prevent |
| [1.3 Give noisy work to a subagent](#13-give-noisy-work-to-a-subagent) | Prevent |
| [1.4 Go back with `/rewind` when an attempt failed](#14-go-back-with-rewind-when-an-attempt-failed) | Remove a dead end |
| [1.5 Compact when a long task must continue](#15-compact-when-a-long-task-must-continue) | Shrink, by hand or by setting |

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

**What:** Give the assistant only the part of a log or a file that matters, and
keep the rules file (for example `CLAUDE.md`) short.

**Why:** Whatever you send stays in the context and goes out again with every
request (see 1.1). The rules file rides along on every request too.

<img alt="Two horizontal bars. With a vague request, the context holds the rules file, your message, a search and six files, and it fills most of the frame. With a precise request that names one file, the context holds the rules file, your message and one file, so it stays small." src="../images/send-only-what-the-task-needs.svg#gh-light-mode-only">
<img alt="Two horizontal bars. With a vague request, the context holds the rules file, your message, a search and six files, and it fills most of the frame. With a precise request that names one file, the context holds the rules file, your message and one file, so it stays small." src="../images/send-only-what-the-task-needs-dark.svg#gh-dark-mode-only">

**How:**
- Point to the file with `@`. Type `@login.ts` and accept the file in the menu,
  instead of writing "find the login code". Claude then has the file and doesn't
  need to search for it.
- If only a part of a file matters, name the lines in your prompt, for example
  "Read lines 40 to 80 of src/login.ts". Claude can read part of a file with an
  `offset` and a `limit`
  ([Claude Code docs](https://code.claude.com/docs/en/tools-reference#read-tool-behavior)).
- Filter a log before Claude sees it. For example, run `tail -n 100 build.log`
  or `grep ERROR build.log | head -50`, and give Claude the result. You can also
  ask Claude to run the filtered command. A hook can trim the output of every
  command for you
  ([Claude Code docs](https://code.claude.com/docs/en/costs#offload-processing-to-hooks-and-skills)).
- Ask for short answers, for example "Be brief. Show only the code that
  changes."
- Keep `CLAUDE.md` or `AGENTS.md` under 200 lines
  ([Claude Code docs](https://code.claude.com/docs/en/memory#write-effective-instructions)).
- Move the instructions of one workflow, for example a pull request review, from
  `CLAUDE.md` to a skill. The body of a skill loads only when the skill is used.
  A skill is also a way to repeat a workflow: you write it once, and you can call
  it by name
  ([Claude Code docs](https://code.claude.com/docs/en/costs#move-instructions-from-claudemd-to-skills)).
- Put the rules for one part of the project where they load only when needed.
  Use a `CLAUDE.md` or `AGENTS.md` in that folder, or a path-scoped rule (a rule
  file with `paths` in its header). Claude loads them when it reads or edits a
  matching file
  ([Claude Code docs](https://code.claude.com/docs/en/memory#how-claude-md-files-load)).
- Disable the MCP servers that you don't use: run `/mcp`. Until Claude uses a
  tool, only the tool names and the server instructions enter the context. So the
  cost is small, but it isn't zero
  ([Claude Code docs](https://code.claude.com/docs/en/costs#reduce-mcp-server-overhead)).

**Where:**
- Terminal and desktop app, Code tab: type `@` in the prompt box to open a menu
  of paths. The content of the file goes into the context, and so does the
  `CLAUDE.md` of its folder
  ([Claude Code docs](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)).
  The terminal and the desktop app use the same `CLAUDE.md`
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
  [AGENTS.md](https://agents.md/) is a shared format for the rules file of coding
  assistants.
- VS Code and JetBrains extensions: select lines in the editor. The extension
  adds a reference with the line range, for example `@src/auth.ts#L1-99`
  ([JetBrains docs](https://code.claude.com/docs/en/jetbrains),
  [VS Code docs](https://code.claude.com/docs/en/vs-code)).

**Watch out:**
- `@` adds the whole file to the context, up to 25,000 tokens by default. To send
  only some lines, select them in your IDE, or name the lines in your prompt
  without `@`
  ([Claude Code docs](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)).
- After a compaction, Claude loads a path-scoped rule or a `CLAUDE.md` of a
  folder again only when it touches a matching file. If a rule must always
  apply, keep it in the `CLAUDE.md` at the project root
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#what-survives-compaction)).

## 1.3 Give noisy work to a subagent

**What:** A subagent is a helper agent with its own context. Hand it work that
produces a lot of output, such as test runs, logs and searches. It sends back only
a short result.

**Why:** Test runs, logs and documentation searches fill the context fast. Run
them in a subagent and they stay out of your main conversation ([Claude Code
docs](https://code.claude.com/docs/en/costs#delegate-verbose-operations-to-subagents)).

<img alt="Two bars for the main context and one for the subagent. Without a subagent, test logs fill the main context. With a subagent, the logs stay in the own context of the subagent, and only its short result goes back." src="../images/subagent-keeps-noise-out.svg#gh-light-mode-only">
<img alt="Two bars for the main context and one for the subagent. Without a subagent, test logs fill the main context. With a subagent, the logs stay in the own context of the subagent, and only its short result goes back." src="../images/subagent-keeps-noise-out-dark.svg#gh-dark-mode-only">

**How:** To write a subagent, see the
[Claude Code docs](https://code.claude.com/docs/en/sub-agents). To run it on a
smaller model or at a lower effort, see
[3.4](lever-3-use-a-smaller-model.md#34-give-simple-subagent-tasks-to-haiku) and
[4.4](lever-4-lower-the-effort.md#44-give-simple-subagent-tasks-a-low-effort).

**Where:**
- Terminal: Claude Code subagents.
- Desktop app, Code tab: the **Background tasks** pane, in the **⋮** menu of the
  title bar, shows the subagents that run
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).

**Watch out:**
- A subagent can't ask you questions. Claude Code removes the `AskUserQuestion`
  tool from every subagent. Permission prompts still reach you in the main
  session. Put everything the subagent needs in its task, or it has to guess
  ([Claude Code docs](https://code.claude.com/docs/en/sub-agents)).
- The requests of a subagent use energy too. A subagent doesn't use the warm
  cache of the main conversation. It starts its own cache, which lasts 5 minutes
  by default
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#which-ttl-each-request-gets)).

## 1.4 Go back with `/rewind` when an attempt failed

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

## 1.5 Compact when a long task must continue

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
- To drop one failed attempt and keep the rest, see 1.4.
- For the best moment to compact, see
  [2.2 Compact before a break longer than your cache time](lever-2-stay-in-the-cache-window.md#22-compact-before-a-break-longer-than-your-cache-time).
- Claude Code also compacts by itself when the context is full enough. You don't
  choose the moment, and it can happen in the middle of a task. The window depends
  on the model and can be very large: the docs give about 967K tokens for Haiku
  5.5.
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
