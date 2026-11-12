[← Lever 4: Lower the effort](lever-4-lower-the-effort.md) &emsp; [All levers](../README.md) &emsp; [Lever 6: Choose a low-carbon region →](lever-6-choose-a-low-carbon-region.md)

# Lever 5: Make fewer round trips

*Reading time: about 8 minutes.*

> [!IMPORTANT]
> Say the task precisely, plan a big one first, and hand Claude a check to run. If a round goes wrong, stop it early, and after two failed corrections start fresh. And don't send requests that a simple tool or an unattended agent could save you.

**Why it matters:** Every request sends the whole conversation again (see
[1.1](lever-1-keep-the-context-short.md#11-start-a-new-conversation-for-each-unrelated-task)),
so a retry always costs more than the first try. The failed reply stays in the
conversation, and the retry carries it along. Each time Claude uses a tool, that's
one more request, with the tool results attached
([Claude Code docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).
A vague request makes things worse: Claude scans more files than it needs
([Claude Code docs](https://code.claude.com/docs/en/costs#write-specific-prompts)).
And a long context makes the answers less reliable anyway (see
[lever 1](lever-1-keep-the-context-short.md)).
There's no energy figure for this lever. The argument rests on counting requests
and tokens, not on measured energy.

<img alt="Three horizontal bars, one for each request, each in a frame. The first attempt holds your request and reply 1. Retry 1 holds your request, reply 1, your note and reply 2. Retry 2 holds your request, reply 1, your note, reply 2, another note and reply 3. Each bar is longer than the one before, because the failed replies stay in the input." src="../images/retry-resends-the-failed-attempt.svg#gh-light-mode-only">
<img alt="Three horizontal bars, one for each request, each in a frame. The first attempt holds your request and reply 1. Retry 1 holds your request, reply 1, your note and reply 2. Retry 2 holds your request, reply 1, your note, reply 2, another note and reply 3. Each bar is longer than the one before, because the failed replies stay in the input." src="../images/retry-resends-the-failed-attempt-dark.svg#gh-dark-mode-only">

| Item | Role |
|---|---|
| [5.1 Tag the file, set the boundaries and state the expected result](#51-tag-the-file-set-the-boundaries-and-state-the-expected-result) | Prevent: say it clearly |
| [5.2 Agree on a plan before Claude edits anything](#52-agree-on-a-plan-before-claude-edits-anything) | Prevent: agree on the approach |
| [5.3 Give Claude a check to run](#53-give-claude-a-check-to-run) | Prevent: let Claude find its own errors |
| [5.4 Press Esc as soon as Claude goes the wrong way](#54-press-esc-as-soon-as-claude-goes-the-wrong-way) | Recover: stop early |
| [5.5 After two failed corrections, run `/clear` and write a better prompt](#55-after-two-failed-corrections-run-clear-and-write-a-better-prompt) | Recover: start fresh |
| [5.6 Use a simple tool when it does the job](#56-use-a-simple-tool-when-it-does-the-job) | Skip the model |
| [5.7 Stop work that runs without you](#57-stop-work-that-runs-without-you) | Skip the hidden requests |

## 5.1 Tag the file, set the boundaries and state the expected result

**What:** In your first request, tag the file Claude should work on, set the
boundaries, and state the expected result. The boundaries say what Claude must keep
and what it must not do. The expected result is how you'll know it worked.
Whatever you do, don't just write "try again" with nothing new in it.

**Why:** A vague request like "improve this codebase" triggers "broad
scanning". Tag the file, and Claude can work with far fewer file reads
([Claude Code docs](https://code.claude.com/docs/en/costs#write-specific-prompts)).

**How:**
- Tag the file with `@` (see
  [1.2](lever-1-keep-the-context-short.md#12-send-only-what-the-task-needs)).
  Claude then has the file and doesn't need to search for it.
- Set the boundaries. Claude can't read these in the code. They say what to keep
  and what to leave alone, for example "no new libraries" or "don't change the
  public API". If you prefer one of several ways to do it, say which.
- State the expected result: how you'll know the task is done. For example, "the
  failing login test passes" or "the total shows two decimals".
- Point to an example, such as "follow the pattern in HotDogWidget.php".
- Hand over what Claude needs: the error text, the failing test, a screenshot.
  Don't describe it, show it
  ([Claude Code docs](https://code.claude.com/docs/en/best-practices#tell-claude-what-it-cant-see)).

**Where:**
- Terminal: type `@` and a path, or paste the error into the prompt.
- Desktop app, Code tab: type `@` followed by a file name in the prompt box
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).

## 5.2 Agree on a plan before Claude edits anything

**What:** For a big task, settle the plan before Claude touches a file. If you
haven't thought the task through yet, ask Claude to interview you and write a spec
(a written description of what to build) first. Then ask for a plan, read it, and
fix what's wrong with it.

**Why:** In the interview, Claude comes back with "multiple-choice questions about
the parts you haven't pinned down"
([Claude Code docs](https://code.claude.com/docs/en/best-practices#brainstorm-with-claude-first)).
A choice that stays open until the build tends to come back later as a retry. In
plan mode, Claude "does not edit your source" until you approve the plan
([Claude Code docs](https://code.claude.com/docs/en/permission-modes#analyze-before-you-edit-with-plan-mode)).
The docs say plan mode prevents "expensive re-work when the initial direction
is wrong"
([Claude Code docs](https://code.claude.com/docs/en/costs#work-efficiently-on-complex-tasks)).

**How:**
- If the task is fuzzy, use a prompt like the one in the docs: "Interview me until
  you could write the spec". Ask Claude to write the spec to a file, for example
  `SPEC.md`.
- Then plan. Press Shift+Tab until the status bar shows `⏸ plan mode on`, or start
  one prompt with `/plan`. You can also launch with
  `claude --permission-mode plan`. When the plan is ready, Claude asks how to
  proceed. Press Ctrl+G to edit the plan in your editor first
  ([Claude Code docs](https://code.claude.com/docs/en/permission-modes#review-and-approve-a-plan)).
- Once the spec and the plan are ready, start a fresh session and build from the
  spec. That way the interview drops out of the context (see
  [1.1](lever-1-keep-the-context-short.md#11-start-a-new-conversation-for-each-unrelated-task)).
  To plan on a larger model and build on a smaller one, see
  [3.3](lever-3-use-a-smaller-model.md#33-plan-with-opus-and-do-the-work-on-sonnet).

**Where:**
- Terminal: Shift+Tab, `/plan` or `--permission-mode plan`.
- Desktop app, Code tab: choose **Plan** with the mode selector in the row under
  the prompt box. It applies to this session only. Shift+Tab doesn't cycle the
  modes there. Cmd+Shift+M opens the permission mode menu (Ctrl+Shift+M on
  Windows)
  ([Claude Code docs](https://code.claude.com/docs/en/desktop#choose-a-permission-mode)).

**Watch out:** The questions and the plan are requests too. Use the interview only
when the task isn't clear, and plan mode only when the task is complex. For a
small task, just ask.

## 5.3 Give Claude a check to run

**What:** Give Claude a test, a linter or a build to run, so it finds its own
mistakes before you have to ask for a fix.

**Why:** When Claude can check its own work, it "catches issues before you need
to request fixes"
([Claude Code docs](https://code.claude.com/docs/en/costs#work-efficiently-on-complex-tasks)).
With a check that gives pass or fail, Claude can "keep going on its own"
([Claude Code docs](https://code.claude.com/docs/en/best-practices#give-claude-a-check-to-run)).

**How:** Name the check in the request, for example "run the tests and fix the
failures". The docs list a few options: a test file, a build's exit code, a
linter, or a script that compares the output to a fixture. Ask Claude to show the
command and its output. For a long task, the docs advise: "Write one file, test
it, then continue."

**Watch out:** A check doesn't make the retries disappear. Claude just makes them
instead of you. Every run of the check and every fix is still a request that sends
the whole conversation again (see the intro). Where it pays off is when the check
catches an error before you read the result, because then you never have to send a
correction. The docs give no token figure for this loop.

## 5.4 Press Esc as soon as Claude goes the wrong way

**What:** When Claude heads down the wrong path, press Esc. Don't wait for it to
finish.

**Why:** Esc "stops Claude mid-action and keeps everything up to that point"
([Claude Code docs](https://code.claude.com/docs/en/best-practices#steer-the-session)).
Each tool use sends one more request (see the intro), so work you stop early
never sends those requests.

**How:** Keep an eye on the first steps. When Claude drifts, press Esc, then send
a precise message (see 5.1). To go back to an earlier point and drop the dead end,
use `/rewind`, or press Esc twice on an empty prompt (see
[1.4](lever-1-keep-the-context-short.md#14-go-back-with-rewind-when-an-attempt-failed)).

**Watch out:** Checkpoints only cover the files that Claude edited with its own
file tools. If a Bash command changed a file, it isn't restored
([Claude Code docs](https://code.claude.com/docs/en/best-practices#steer-the-session)).

## 5.5 After two failed corrections, run `/clear` and write a better prompt

**What:** If you've corrected the same thing twice and it's still wrong, don't send
a third correction. Run `/clear` and write a new prompt with what you've learned.

**Why:** A third correction "mostly adds to a conversation that already contains
two failed attempts". The docs say that `/clear` plus one new prompt that includes
what you learned works better almost every time
([Claude Code docs](https://code.claude.com/docs/en/best-practices#steer-the-session)).

<img alt="Two horizontal bars, each in a frame. The first bar is the request after a third correction. It holds your request, three failed replies and three fixes as input, and a new reply 4 as output. The second bar is the request after /clear with a better prompt. It holds only the better prompt as input and a new reply as output. The second bar is much shorter." src="../images/fresh-start-beats-a-third-correction.svg#gh-light-mode-only">
<img alt="Two horizontal bars, each in a frame. The first bar is the request after a third correction. It holds your request, three failed replies and three fixes as input, and a new reply 4 as output. The second bar is the request after /clear with a better prompt. It holds only the better prompt as input and a new reply as output. The second bar is much shorter." src="../images/fresh-start-beats-a-third-correction-dark.svg#gh-dark-mode-only">

**How:** Put what the failed tries showed you into the new prompt: the file, the
error and the boundaries (see 5.1). `/clear` costs nothing. To find the old session
later, run `/rename` before `/clear`, and `/resume` to go back to it
([Claude Code docs](https://code.claude.com/docs/en/costs#manage-context-proactively)).
If part of the conversation is good, use `/rewind` instead (see
[1.4](lever-1-keep-the-context-short.md#14-go-back-with-rewind-when-an-attempt-failed)).

**Where:**
- Terminal: `/clear`.
- Desktop app, Code tab: the Desktop docs don't list `/clear`. To start a new
  session, click **+ New** or press Cmd+N (Ctrl+N on Windows)
  ([Claude Code docs](https://code.claude.com/docs/en/desktop)).

## 5.6 Use a simple tool when it does the job

**What:** If a search, a rename, a formatter or a linter can do the task, use it
instead of writing a prompt.

**Why:** Every prompt starts requests that send the whole conversation (see the
intro). The Claude Code docs give two cases where a plain tool costs less
context. CLI tools such as `gh` and `aws` are "more context-efficient than MCP
servers"
([Claude Code docs](https://code.claude.com/docs/en/costs#reduce-mcp-server-overhead)).
And for typed languages, one "go to definition" call replaces a grep plus the
reading of several files
([Claude Code docs](https://code.claude.com/docs/en/costs#install-code-intelligence-plugins-for-typed-languages)).

**How:**
- Use your editor's search or `grep` to find code, your editor's rename to rename
  a symbol, and run your formatter and linter yourself.
- For a typed language, install a code intelligence plugin.
- Want Claude to tell you when a tool could do the job? Add the matching line of
  the [permanent rules](../README.md#permanent-rules) to your rules file.

**Watch out:** No energy figure compares a simple tool with a model request. The
reason above is fewer tokens, not a measured energy saving.

## 5.7 Stop work that runs without you

**What:** Agent teams, scheduled tasks and idle check-ins send requests while you're
doing nothing. Use them only when you need them, keep them small, and stop them
when the work is done.

**Why:**
- In an agent team, each teammate is a separate Claude instance with its own
  context. A team uses about 7 times more tokens than a standard session when the
  teammates run in plan mode
  ([Claude Code docs](https://code.claude.com/docs/en/costs#manage-agent-team-costs)).
  Each active teammate keeps using tokens until it exits
  ([Claude Code docs](https://code.claude.com/docs/en/costs#agent-team-token-costs)).
- A session-scoped scheduled task fires on its interval even when the session is
  idle, and it sends your full context each time. A goal check-in does the same, at
  most three times between your prompts
  ([Claude Code docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).

**How:**
- Agent teams are off by default. Turn them on only for work that really needs
  them.
- Use Sonnet for the teammates, keep the team small, and give each teammate a
  small task that stands alone. Shut a teammate down when its work is done
  ([Claude Code docs](https://code.claude.com/docs/en/costs#agent-team-token-costs)).
- To stop the idle goal check-ins, set `CLAUDE_CODE_GOAL_CHECKIN_MINUTES` to `0`.
  Idle check-ins need Claude Code 2.1.236 or later.
- Stop a scheduled task as soon as you don't need it any more.

**Watch out:** The figure of 7 times is for teammates in plan mode. The docs give
no figure for other cases.

---

[← Lever 4: Lower the effort](lever-4-lower-the-effort.md) &emsp; [All levers](../README.md) &emsp; [Lever 6: Choose a low-carbon region →](lever-6-choose-a-low-carbon-region.md)
