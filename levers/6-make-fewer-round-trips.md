[← 5 · Cut tools and side work](5-cut-tools-and-side-work.md) · [All levers](../README.md) · [7 · Choose a low-carbon region →](7-choose-a-low-carbon-region.md)

# 6 · Make fewer round trips

**In one line:** ask precisely, plan large tasks first, and do not use AI when a
simple tool does the job.

## Why it matters

- Each failed attempt is a new round of requests, and each request sends the
  full conversation again
  ([Claude Code docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).
  A retry costs more than the first attempt, because the context is longer.
- A precise request (files, limits, expected result) needs fewer rounds than a
  vague one.
- Search, rename, formatting and code from templates do not need a model. Your
  editor, a formatter or a linter does these tasks with almost no energy.

## How to apply

**All assistants**
- Say which files, which limits and what result you expect.
- For a large task, ask for a plan first. Correct the plan, then let the
  assistant work. In Claude Code, use plan mode.
- Do not say "try again" without new information. Give the error, the test that
  fails, or what is wrong.
- Before you ask, think: can my editor, `grep`, a formatter or a linter do it?

**Teams:** add the project commands (test, lint, format) to the rules file, so
the assistant uses them instead of guessing.

## Copy this

Before a large task:

```text
Before you write code, describe the task in 3 lines, list the files that you will change, and ask me the questions that you need. Then make the smallest change that solves the task.
```

When you are not sure that you need AI:

```text
Does this task need AI? If a standard tool can do it (search, formatter, linter, code generator, refactoring tool), tell me which one and how to use it.
```

## Check it

- Count the rounds of a typical task. If you often need more than 3, make the
  first request more precise.

## Resources

- [Manage costs effectively](https://code.claude.com/docs/en/costs) (Claude
  Code docs): see [Write specific prompts](https://code.claude.com/docs/en/costs#write-specific-prompts)
  and [Work efficiently on complex tasks](https://code.claude.com/docs/en/costs#work-efficiently-on-complex-tasks).

---

[← 5 · Cut tools and side work](5-cut-tools-and-side-work.md) · [All levers](../README.md) · [7 · Choose a low-carbon region →](7-choose-a-low-carbon-region.md)
