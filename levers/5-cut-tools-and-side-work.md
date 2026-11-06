[← 4 · Stay in the cache window](4-stay-in-the-cache-window.md) · [All levers](../README.md) · [6 · Make fewer round trips →](6-make-fewer-round-trips.md)

# 5 · Cut tools and side work

**In one line:** disconnect the tools that you do not use, and give noisy work
(tests, logs, searches) to a subagent on a small model.

## Why it matters

- **Tools:** each connected tool server (MCP server) adds its tool names and
  instructions to the context
  ([Claude Code docs](https://code.claude.com/docs/en/costs#reduce-mcp-server-overhead)).
  Command-line tools such as `gh`, `aws` or `gcloud` do not add a tool list to
  the context. The Claude Code docs call them "more context-efficient than MCP
  servers".
- **Side work:** test runs, logs and documentation searches fill the context
  fast. A subagent does this work in its own context and sends back only a short
  result ([Claude Code docs](https://code.claude.com/docs/en/costs#delegate-verbose-operations-to-subagents)).
  A subagent still makes its own requests, so give it a small model
  ([Claude Code docs](https://code.claude.com/docs/en/costs#choose-the-right-model)).

## How to apply

**Claude Code**
- See the servers and disconnect the ones that you do not use: `/mcp`.
- See what the tools use in the context: `/context`.
- Prefer a command-line tool when it exists (`gh` for GitHub, for example).
- For simple subagent tasks, set `model: haiku` in the subagent file (see
  below).

**Other assistants:** disconnect the extensions and tool servers that you do not
use for the current project.

**Teams:** share only the tool servers that the project needs, in the project
configuration. Share the subagents too (in Claude Code: `.claude/agents/`).

## Copy this

A subagent that runs the tests on a small model. Save it as
`.claude/agents/test-runner.md`:

```markdown
---
name: test-runner
description: Run the tests and report only the result. Use after each code change.
model: haiku
---

Run the test suite. Report the number of passed and failed tests, and the first
error of each failed test. Do not copy the full output.
```

## Check it

- `/context` before and after you disconnect a server.
- `/usage` shows the tokens for each model, so you can see the work done by the
  small model.

## Resources

- [Reduce MCP server overhead](https://code.claude.com/docs/en/costs#reduce-mcp-server-overhead)
  (Claude Code docs).
- [Subagents](https://code.claude.com/docs/en/sub-agents) (Claude Code docs):
  how to write a subagent and choose its model.

---

[← 4 · Stay in the cache window](4-stay-in-the-cache-window.md) · [All levers](../README.md) · [6 · Make fewer round trips →](6-make-fewer-round-trips.md)
