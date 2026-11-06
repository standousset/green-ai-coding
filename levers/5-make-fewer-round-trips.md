[← 4 · Lower the effort](4-lower-the-effort.md) · [All levers](../README.md) · [6 · Choose a low-carbon region →](6-choose-a-low-carbon-region.md)

# 5 · Make fewer round trips

**In one line:** ask precisely, plan large tasks first, and do not use AI when a
simple tool does the job.

## A retry costs more than the first attempt

Each failed attempt is a new round of requests, and each request sends the full
conversation again
([Claude Code docs](https://code.claude.com/docs/en/costs#why-usage-climbs-in-a-long-session)).
A retry costs more than the first attempt, because the context is longer.

Count the rounds of a typical task. As a rule of thumb (not a measured limit),
if you often need more than 3, make the first request more precise.

## Ask precisely

A precise request needs fewer rounds than a vague one. Say which files, which
limits and what result you expect. Do not say "try again" without new
information: give the error, the test that fails, or what is wrong
([Claude Code docs](https://code.claude.com/docs/en/costs#write-specific-prompts)).

## Plan large tasks first

For a large task, ask for a plan first. Correct the plan, then let the assistant
work. Use the plan mode if your assistant has one
([Claude Code docs](https://code.claude.com/docs/en/costs#work-efficiently-on-complex-tasks)).

## Do not use AI when a simple tool does the job

Search, rename, formatting and code from templates do not need a model. Your
editor, `grep`, a formatter or a linter does these tasks with far less energy.
Before you ask, check if one of them can do the task.

---

[← 4 · Lower the effort](4-lower-the-effort.md) · [All levers](../README.md) · [6 · Choose a low-carbon region →](6-choose-a-low-carbon-region.md)
