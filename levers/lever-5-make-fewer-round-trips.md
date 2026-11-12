[← Lever 4: Lower the effort](lever-4-lower-the-effort.md) &emsp; [All levers](../README.md) &emsp; [Lever 6: Choose a low-carbon region →](lever-6-choose-a-low-carbon-region.md)

# Lever 5: Make fewer round trips

> [!IMPORTANT]
> Say the task precisely, plan a large task first, stop a wrong path early, and use a simple tool when it does the job.

**Why it matters:** Each request sends the whole conversation again (see
[1.1](lever-1-keep-the-context-short.md#11-drop-old-context-that-the-task-does-not-need)),
so a retry costs more than the first attempt. A vague request also makes Claude
scan more files than it needs
([Claude Code docs](https://code.claude.com/docs/en/costs#write-specific-prompts)).
A long context makes the answers less reliable too (see
[lever 1](lever-1-keep-the-context-short.md)).

| Item | Role |
|---|---|
| [5.1 Know that a retry costs more than the first attempt](#51-know-that-a-retry-costs-more-than-the-first-attempt) | Learn the rule |
| [5.2 Name the file, the limit and the result in your first request](#52-name-the-file-the-limit-and-the-result-in-your-first-request) | Prevent: say it clearly |
| [5.3 Ask Claude to interview you when the task is not clear](#53-ask-claude-to-interview-you-when-the-task-is-not-clear) | Prevent: find out what you want |
| [5.4 Plan a large task first](#54-plan-a-large-task-first) | Prevent: agree on the approach |
| [5.5 Give Claude a check to run](#55-give-claude-a-check-to-run) | Prevent: let Claude find its own errors |
| [5.6 Press Esc as soon as Claude goes the wrong way](#56-press-esc-as-soon-as-claude-goes-the-wrong-way) | Recover: stop early |
| [5.7 After two failed corrections, run `/clear` and write a better prompt](#57-after-two-failed-corrections-run-clear-and-write-a-better-prompt) | Recover: start fresh |
| [5.8 Use a simple tool when it does the job](#58-use-a-simple-tool-when-it-does-the-job) | Skip the model |

## 5.1 Know that a retry costs more than the first attempt

**What:** Count the rounds that a typical task needs. Each retry is a new request
with a longer context.

**Why:** Each request sends the full conversation again. Each time Claude uses a
tool, it sends one more request with the tool results
([Claude Code docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).
A failed reply stays in the conversation, so the retry sends it again.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../images/retry-resends-the-failed-attempt-dark.svg">
  <img alt="Three horizontal bars, one for each request, each in a frame. The first attempt holds your request and reply 1. Retry 1 holds your request, reply 1, your note and reply 2. Retry 2 holds your request, reply 1, your note, reply 2, another note and reply 3. Each bar is longer than the one before, because the failed replies stay in the input." src="../images/retry-resends-the-failed-attempt.svg">
</picture>

**How:** To get the result in fewer rounds, use 5.2 to 5.5. When a round fails,
use 5.6 and 5.7.

**Where:**
- Terminal: the rule above.
- Chat: a long chat pushes you toward the usage limit
  ([Claude help center](https://support.claude.com/en/articles/11647753-how-do-usage-and-length-limits-work)).

## 5.2 Name the file, the limit and the result in your first request

**What:** Say which file, which limit and which result you expect. Do not write
"try again" with no new information.

**Why:** A vague request such as "improve this codebase" triggers "broad
scanning". A request that names the file lets Claude work with few file reads
([Claude Code docs](https://code.claude.com/docs/en/costs#write-specific-prompts)).

**How:**
- Point to the file with `@` (see
  [1.2](lever-1-keep-the-context-short.md#12-send-only-what-the-task-needs)).
- State the limits that Claude cannot read in the code, for example "no new
  libraries". Say which choice you prefer, up front.
- Point to an example, such as "follow the pattern in HotDogWidget.php".
- Hand over what Claude needs, such as the error text, the failing test or a
  screenshot. Do not describe it
  ([Claude Code docs](https://code.claude.com/docs/en/best-practices#tell-claude-what-it-cant-see)).

**Where:**
- Terminal: type `@` and a path, or paste the error into the prompt.
- Desktop app, Code tab: type `@` followed by a file name in the prompt box
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).

## 5.3 Ask Claude to interview you when the task is not clear

**What:** If you have not thought the task through, ask Claude to question you
first and to write a spec.

**Why:** Claude comes back with "multiple-choice questions about the parts you
haven't pinned down"
([Claude Code docs](https://code.claude.com/docs/en/best-practices#brainstorm-with-claude-first)).
A choice that stays open until the build can come back later as a retry (see 5.1).

**How:**
- Use a prompt like the one in the docs. It says: "Interview me until you could
  write the spec".
- Ask Claude to write the spec to a file, for example `SPEC.md`.
- When the spec exists, start a fresh session to build from it. The interview
  then leaves the context (see
  [1.1](lever-1-keep-the-context-short.md#11-drop-old-context-that-the-task-does-not-need)).

**Where:**
- Terminal: a normal prompt.

**Watch out:** The questions are requests too. Use this only when the task is not
clear.

## 5.4 Plan a large task first

**What:** For a large task, ask for a plan first. Read it, correct it, then let
Claude edit.

**Why:** In plan mode, Claude "does not edit your source" until you approve the
plan
([Claude Code docs](https://code.claude.com/docs/en/permission-modes#analyze-before-you-edit-with-plan-mode)).
The Claude Code docs say that plan mode prevents "expensive re-work when the
initial direction is wrong"
([source](https://code.claude.com/docs/en/costs#work-efficiently-on-complex-tasks)).

**How:** Press Shift+Tab until the status bar shows `⏸ plan mode on`, or start
one prompt with `/plan`. You can also start with `claude --permission-mode plan`.
When the plan is ready, Claude asks how to proceed. Press Ctrl+G to edit the plan
in your editor before Claude continues
([Claude Code docs](https://code.claude.com/docs/en/permission-modes#review-and-approve-a-plan)).

**Where:**
- Terminal: the steps above.
- Desktop app, Code tab: choose **Plan** with the mode selector in the row under
  the prompt box. It applies to this session only. Shift+Tab does not cycle the
  modes there. Cmd+Shift+M opens the permission mode menu (Ctrl+Shift+M on
  Windows)
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#choose-a-permission-mode)).

**Watch out:** The docs recommend plan mode for complex tasks. A plan is a round
of requests too, so for a small task, ask directly.

## 5.5 Give Claude a check to run

**What:** Give Claude a test, a linter or a build to run, so it finds its own
errors before you ask for a fix.

**Why:** When Claude can check its own work, it "catches issues before you need
to request fixes"
([Claude Code docs](https://code.claude.com/docs/en/costs#work-efficiently-on-complex-tasks)).
With a check that gives pass or fail, Claude can "keep going on its own"
([Claude Code docs](https://code.claude.com/docs/en/best-practices#give-claude-a-check-to-run)).

**How:** Name the check in the request, for example "run the tests and fix the
failures". The docs list these checks: a test file, a build's exit code, a
linter, and a script that compares the output to a fixture. Ask Claude to show the
command and its output. For a long task, the docs advise: "Write one file, test
it, then continue."

**Where:**
- Terminal: the steps above.

**Watch out:** This moves the retry into the loop of Claude. Each run and each fix
is still a request that sends the whole conversation (see 5.1). It saves your
prompts. The docs give no figure for the tokens.

## 5.6 Press Esc as soon as Claude goes the wrong way

**What:** When Claude starts on a wrong path, press Esc. Do not wait for it to
finish.

**Why:** Esc "stops Claude mid-action and keeps everything up to that point"
([Claude Code docs](https://code.claude.com/docs/en/best-practices#steer-the-session)).
Each tool use sends one more request (see 5.1), so work that you stop early does
not send those requests.

**How:** Watch the first minute. When Claude drifts, press Esc, and then send a
precise message (see 5.2). To go back to an earlier point and drop the dead end,
use `/rewind`, or press Esc twice on an empty prompt (see
[1.4](lever-1-keep-the-context-short.md#14-go-back-with-rewind-when-a-path-did-not-work)).

**Where:**
- Terminal: Esc and `/rewind`.

**Watch out:** The checkpoints cover only the files that Claude edited with its
own file tools. A file that a Bash command changed is not restored
([Claude Code docs](https://code.claude.com/docs/en/best-practices#steer-the-session)).

## 5.7 After two failed corrections, run `/clear` and write a better prompt

**What:** If you corrected the same thing twice and it is still wrong, do not send
a third correction. Run `/clear` and write a new prompt with what you learned.

**Why:** A third correction "mostly adds to a conversation that already contains
two failed attempts". The docs say that `/clear` and one new prompt that has what
you learned works better almost every time
([Claude Code docs](https://code.claude.com/docs/en/best-practices#steer-the-session)).

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../images/fresh-start-beats-a-third-correction-dark.svg">
  <img alt="Two horizontal bars, each in a frame. The first bar is the request after a third correction. It holds your request, three failed replies and three fixes as input, and a new reply 4 as output. The second bar is the request after /clear with a better prompt. It holds only the better prompt as input and a new reply as output. The second bar is much shorter." src="../images/fresh-start-beats-a-third-correction.svg">
</picture>

**How:** Put what the failed tries showed into the new prompt: the file, the error
and the limit (see 5.2). `/clear` costs nothing. To find the old session later, run
`/rename` before `/clear`, and `/resume` to return to it
([Claude Code docs](https://code.claude.com/docs/en/costs#manage-context-proactively)).
If part of the conversation is good, use `/rewind` instead (see
[1.4](lever-1-keep-the-context-short.md#14-go-back-with-rewind-when-a-path-did-not-work)).

**Where:**
- Terminal: `/clear`.
- Desktop app, Code tab: the Desktop docs do not list `/clear`. To start a new
  session, click **+ New** or press Cmd+N (Ctrl+N on Windows)
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).
- Chat: start a new chat
  ([Claude help center](https://support.claude.com/en/articles/11647753-how-do-usage-and-length-limits-work)).

## 5.8 Use a simple tool when it does the job

**What:** If a search, a rename, a formatter or a linter can do the task, use it
and do not write a prompt.

**Why:** Each prompt starts requests that send the whole conversation (see 5.1).
The Claude Code docs give two cases where a plain tool costs less context. CLI
tools such as `gh` and `aws` are "more context-efficient than MCP servers"
([Claude Code docs](https://code.claude.com/docs/en/costs#reduce-mcp-server-overhead)).
For typed languages, one "go to definition" call replaces a grep and the reading
of several files
([Claude Code docs](https://code.claude.com/docs/en/costs#install-code-intelligence-plugins-for-typed-languages)).

**How:**
- Use the search of your editor or `grep` to find code. Use the rename of your
  editor to rename a symbol. Run your formatter and your linter yourself.
- For a typed language, install a code intelligence plugin.
- To have Claude tell you when a tool can do the task, add the matching line of
  the [permanent rules](../README.md#b---copy-this-permanent-rules) to your rules
  file.

**Where:**
- Terminal: the steps above.

**Watch out:** This guide has no energy figure that compares a simple tool with a
model request. The reason above is fewer tokens, not a measured energy saving.

---

[← Lever 4: Lower the effort](lever-4-lower-the-effort.md) &emsp; [All levers](../README.md) &emsp; [Lever 6: Choose a low-carbon region →](lever-6-choose-a-low-carbon-region.md)
