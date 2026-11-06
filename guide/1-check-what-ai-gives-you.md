# 1 · Check what AI gives you

**Space:** How you choose and use AI
**Principle:** 🔍 Trustworthy · *Can I check it?*

## Why it matters

An assistant writes code and explanations that look correct, even when they are
wrong. It can invent functions, options and API behaviour, and present them with
the same confidence as real facts.

## What to do

### Everyone

- Ask the assistant to explain each change in simple words before you accept it. If the explanation is not clear, do not accept the change.
- Run the result and try it yourself, also with wrong inputs.
- Work in small steps: one feature or one fix for each request.

### Developers

- Read the full set of changes (the diff) before you accept it. Reject what you cannot explain.
- When the assistant states a fact about a library or an API, check it in the official documentation for the version that you use.
- Ask the assistant to review its own answer for faults, then to correct them.

### Teams and architects

- Put a shared rules file (for example `AGENTS.md`) in each repository, with the conventions of the project, so that assistants follow them.
- Apply the same review rules to AI-assisted changes as to all other changes.

## Resources

- [Prompts for this part](../toolkit/prompts.md#1--check-what-ai-gives-you): ready to copy into your assistant.
- [Keep the context small](4-use-only-the-ai-you-need.md#3-keep-the-context-small): a long context makes answers less reliable.
- [AGENTS.md](https://agents.md/): an open format for a rules file that many
  coding assistants read.
- [Security-Focused Guide for AI Code Assistant Instructions](https://best.openssf.org/Security-Focused-Guide-for-AI-Code-Assistant-Instructions)
  (OpenSSF): includes the self-review technique ("Recursive Criticism and
  Improvement") and how to ask for it in your instructions.

## Not covered here

- Invented packages, secrets and permissions: see [2 · Protect your environment](2-protect-your-environment.md).
- Reliability of the code that you ship: see [6 · Reliable code](6-reliable-code.md).

---

[Index](../README.md) · Next: [2 · Protect your environment](2-protect-your-environment.md)
