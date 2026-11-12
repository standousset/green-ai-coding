[All levers](../README.md) &emsp; [Lever 2: Stay in the cache window →](lever-2-stay-in-the-cache-window.md)

# Lever 1: Keep the context short

> [!IMPORTANT]
> Give the assistant only what the task needs, give noisy work to a subagent, and compact when a long task must continue.

**Why it matters:** A long context means many more tokens, because each request
sends the whole conversation again (see 1.1). It also makes the answers less reliable. In a
study of 18 models, the results became less reliable as the input got longer
([Chroma, Context Rot](https://www.trychroma.com/research/context-rot)). Less
reliable answers lead to more retries (see
[lever 5](lever-5-make-fewer-round-trips.md)).

| Item | Role |
|---|---|
| [1.1 Drop old context that the task does not need](#11-drop-old-context-that-the-task-does-not-need) | Start clean |
| [1.2 Send only what the task needs](#12-send-only-what-the-task-needs) | Prevent |
| [1.3 Give noisy work to a subagent](#13-give-noisy-work-to-a-subagent) | Prevent |
| [1.4 Go back with `/rewind` when a path did not work](#14-go-back-with-rewind-when-a-path-did-not-work) | Remove a dead end |
| [1.5 Compact when a long task must continue](#15-compact-when-a-long-task-must-continue) | Shrink |
| [1.6 Set when auto-compaction starts](#16-set-when-auto-compaction-starts) | Set the automatic fallback |

## 1.1 Drop old context that the task does not need

**What:** Keep only the conversation that the current task needs. Start a new
conversation for an unrelated task.

**Why:** Each request sends the full conversation again. Each time the assistant
uses a tool, it sends one more request with the tool results
([Claude Code docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).
Old, unrelated context adds to the cost of each message.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../images/context-grows-each-request-dark.svg">
  <img alt="Four horizontal bars, one for each request, each in a frame that shows the maximum context size. Each bar shows the input, then the new output. The reply of one request is part of the input of the next request, so the bars grow longer and fill more of the frame." src="../images/context-grows-each-request.svg">
</picture>

**How:** Use `/context` to see what fills the context. Use `/clear` to start a
new conversation between unrelated tasks.

**Where:**
- Terminal: `/context` shows what fills the context. `/clear` starts a new
  conversation.
- Desktop app, Code tab: type `/context` in the prompt box. The session details
  also show how full the context window is. To start a new session, click
  **+ New** or press Cmd+N (Ctrl+N on Windows)
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
- Chat: start a new chat when a long chat pushes you toward the usage limit
  ([Claude help center](https://support.claude.com/en/articles/11647753-how-do-usage-and-length-limits-work)).

## 1.2 Send only what the task needs

**What:** Give the assistant only the part of a log or a file that matters, and
keep the rules file short.

**Why:** What you send stays in the context and goes again with each request (see
1.1). The rules file is part of every request.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../images/send-only-what-the-task-needs-dark.svg">
  <img alt="Two horizontal bars. With a vague request, the context holds the rules file, your message, a search and six files, and it fills most of the frame. With a precise request that names one file, the context holds the rules file, your message and one file, so it stays small." src="../images/send-only-what-the-task-needs.svg">
</picture>

**How:**
- Point to the file with `@`. Type `@login.ts` and accept the file in the menu,
  and not "find the login code". Claude then has the file and does not need to
  search for it.
- Give only the part of a log or a file that matters.
- Ask for short answers, for example "Be brief. Show only the code that
  changes."
- Keep `CLAUDE.md` or `AGENTS.md` under 200 lines
  ([Claude Code docs](https://code.claude.com/docs/en/memory#write-effective-instructions)).
- Move specialized instructions to a skill. The body of a skill loads only when
  the skill is used
  ([Claude Code docs](https://code.claude.com/docs/en/costs#move-instructions-from-claudemd-to-skills)).
- Put the rules for one part of the project in a path-scoped rule (a rule file
  with `paths` in its header). It loads only when Claude uses a matching file
  ([Claude Code docs](https://code.claude.com/docs/en/memory#path-specific-rules)).
- Disable the MCP servers that you do not use: run `/mcp`. By default, only the
  tool names and the server instructions enter the context until Claude uses a
  tool, so the cost is small, but it is not zero
  ([Claude Code docs](https://code.claude.com/docs/en/costs#reduce-mcp-server-overhead)).

**Where:**
- Terminal: type `@` to open a menu of paths. The content of the file goes into
  the context, and so does the `CLAUDE.md` of its folder
  ([Claude Code docs](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)).
- Desktop app, Code tab: type `@` followed by a file name in the prompt box
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
- Terminal and desktop app, Code tab: `CLAUDE.md` is the rules file of Claude
  Code, and the two share it
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
  [AGENTS.md](https://agents.md/) is a shared format for the rules file of coding
  assistants.
- Chat: a project loads only the content that is relevant, so keep the project
  instructions short and remove the files that you do not use. Turn off the
  connected apps that you do not need. Ask Claude not to search the web when you
  do not need current information
  ([Claude help center](https://support.claude.com/en/articles/11647753-how-do-usage-and-length-limits-work)).

**Watch out:**
- `@` adds the whole file to the context, up to 25,000 tokens by default. For a
  large file, give only the part that matters.
- An `@` import inside `CLAUDE.md` loads at launch, together with the file that
  names it. It does not make the context smaller
  ([Claude Code docs](https://code.claude.com/docs/en/memory#import-additional-files)).

## 1.3 Give noisy work to a subagent

**What:** A subagent is a helper agent with its own context. It does the noisy
work and sends back only a short result.

**Why:** Test runs, logs and documentation searches fill the context fast. In
the context of a subagent, they stay out of your main conversation
([Claude Code docs](https://code.claude.com/docs/en/costs#delegate-verbose-operations-to-subagents)).

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../images/subagent-keeps-noise-out-dark.svg">
  <img alt="Two bars for the main context and one for the subagent. Without a subagent, test logs fill the main context. With a subagent, the logs stay in the own context of the subagent, and only its short result goes back." src="../images/subagent-keeps-noise-out.svg">
</picture>

**How:** To write a subagent, see the
[Claude Code docs](https://code.claude.com/docs/en/sub-agents). To run it on a
smaller model or at a lower effort, see
[3.7](lever-3-use-a-smaller-model.md#37-give-simple-subagent-tasks-to-haiku) and
[4.7](lever-4-lower-the-effort.md#47-give-simple-subagent-tasks-a-low-effort).

**Where:**
- Terminal: Claude Code subagents.
- Desktop app, Code tab: the **Background tasks** pane, in the **⋮** menu of the
  title bar, shows the subagents that run
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).

**Watch out:**
- A subagent cannot ask you questions. Claude Code removes the
  `AskUserQuestion` tool from every subagent. Permission prompts still reach you
  in the main session. Put everything the subagent needs in its task, or it
  must guess
  ([Claude Code docs](https://code.claude.com/docs/en/sub-agents)).
- The requests of a subagent use energy too. A subagent does not use the warm
  cache of the main conversation. It starts its own cache, which lasts 5 minutes
  by default
  ([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#which-ttl-each-request-gets)).

## 1.4 Go back with `/rewind` when a path did not work

**What:** `/rewind` goes back to an earlier message in the conversation.

**Why:** `/rewind` cuts the conversation back to an earlier turn, so the dead end
leaves the context. The part before that turn is already in the cache, and it
stays there
([Claude Code docs](https://code.claude.com/docs/en/prompt-caching#rewinding-the-conversation)).

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../images/rewind-drops-dead-end-dark.svg">
  <img alt="Two horizontal bars. Before /rewind, the bar holds your message 1 and reply 1, then a rewind point, then the path that did not work. The part before the rewind point stays in the cache. After /rewind, only that part remains, and the next request continues from it." src="../images/rewind-drops-dead-end.svg">
</picture>

**How:** Type `/rewind`, then choose the earlier message.

**Where:**
- Terminal: `/rewind`.
- Desktop app, Code tab: the Desktop docs do not list `/rewind`.

## 1.5 Compact when a long task must continue

**What:** `/compact` replaces the old conversation with a short summary.

**Why:**
- Each request sends the whole conversation again (see 1.1). In a long task,
  every request is large.
- `/compact` replaces the old messages with a summary. Each later request sends
  the summary, not the full conversation.
- Example: 100K tokens of conversation become a summary of 5K tokens. The next
  requests send about 5K tokens plus the new messages, and not 100K. The numbers
  are an example, not a measure.
- The saving repeats on every later request. The longer the task continues, the
  more you save.
- The summary costs one request. Claude Code sends the full conversation once
  more, and the model writes the summary. Compacting a large context is itself a
  large request
  ([Claude Code docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).
- The summary keeps less than the full conversation. Your project `CLAUDE.md` is
  read from disk again, so it is not lost
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#what-survives-compaction)).

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../images/compact-shrinks-context-dark.svg">
  <img alt="Two horizontal bars. Before /compact, five messages and five replies fill most of the frame. After /compact, one short summary block replaces them, and the frame has room again." src="../images/compact-shrinks-context.svg">
</picture>

**How:** When a long task must continue, run `/compact` instead of carrying the
full conversation.
- To choose what the summary keeps, add instructions: `/compact focus on the
  auth bug fix`
  ([Claude Code docs](https://code.claude.com/docs/en/context-window#when-your-context-fills-up)).
- When the next task is unrelated, use `/clear` (see 1.1). It costs nothing.
- To drop one dead end and keep the rest, see 1.4.
- For the best moment to compact, see
  [2.7 Compact while the cache is warm](lever-2-stay-in-the-cache-window.md#27-compact-while-the-cache-is-warm).

**Where:**
- Terminal: `/compact`, or `/compact` with instructions.
- Desktop app, Code tab: type `/compact` in the prompt box. When the context
  fills up, Claude also summarizes the conversation by itself
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
- Chat: there is no command. If code execution is on, Claude summarizes the
  earlier messages by itself when the chat nears the context limit, and keeps the
  full history. A long chat that triggers this uses more of your usage limit, so
  start a new chat at a natural break
  ([Claude help center](https://support.claude.com/en/articles/11647753-how-do-usage-and-length-limits-work)).

## 1.6 Set when auto-compaction starts

**What:** Choose the context size at which Claude Code compacts by itself.

**Why:** Claude Code compacts by itself when the context is full enough. You do
not choose the moment, and it can happen in the middle of a task. The window
depends on the model and can be very large: the docs give about 967K tokens for
Haiku 5.5.

**How:** To compact earlier, use `/autocompact 200k` (or the `autoCompactWindow`
setting). `/autocompact auto` returns to the default.

**Where:**
- Terminal: Claude Code 2.1.221 or later
  ([Claude Code docs](https://code.claude.com/docs/en/model-config#set-the-auto-compact-window)).
- Desktop app, Code tab: Claude compacts by itself when the context is full. The
  Desktop docs do not list `/autocompact`
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
- Chat: the compaction is automatic. The help center gives no setting for it
  ([Claude help center](https://support.claude.com/en/articles/11647753-how-do-usage-and-length-limits-work)).

---

[All levers](../README.md) &emsp; [Lever 2: Stay in the cache window →](lever-2-stay-in-the-cache-window.md)
