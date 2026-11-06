# 7 · Secure code

**Space:** What AI changes in your output
**Principle:** 🔒 Secure · *Can it be attacked?*

## Why it matters

Assistants learn from public code, so they also repeat its common security
faults: insecure defaults, missing input checks, weak use of cryptography. They
can also add dependencies that are vulnerable, or that do not exist.

If your product itself calls an AI model, it has new risks: users or documents
can inject instructions into the model, and the model output can reach parts of
your system that trust it too much
([OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/)).

## What to do

### Everyone

- Do not publish an app that handles logins, payments or personal data before a person who knows security checks it.
- Ask the assistant to list the security risks of its change, then to correct them.
- Turn on automatic alerts for vulnerable dependencies, if your platform has them.

### Developers

- Put security instructions in your assistant's rules file. The OpenSSF guide below gives instructions that you can copy.
- Run static analysis (tools that find faults in code without running it) and a dependency audit (a check of your packages against lists of known vulnerabilities) on every change.
- If your product calls a model: treat its output as untrusted input, and give it the minimum permissions.

### Teams and architects

- Make security checks stop the merge when they fail, in the pipeline.
- For products that use a model, make a threat model that includes prompt injection and data leaks. The OWASP list below is a good start.

## Resources

- [Prompts for this part](../toolkit/prompts.md#7--secure-code): ready to copy into your assistant.
- [Security-Focused Guide for AI Code Assistant Instructions](https://best.openssf.org/Security-Focused-Guide-for-AI-Code-Assistant-Instructions)
  (OpenSSF): security rules for assistant instruction files, including supply
  chain and language-specific points.

  > **In short** (checked 2026-11-06): you stay responsible for the code, and AI-made code still needs
  > review, tests and static analysis. Assume it can be insecure. Put security rules in the assistant's
  > instructions: validate input, encode output, use parameterized queries, keep secrets out of code.
  > Check every dependency, because assistants can suggest packages that do not exist. Ask the
  > assistant to find problems in its own answer, then to improve it.

- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/): the
  risks to check if your product uses a model.
- [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/):
  finds dependencies with known vulnerabilities.

## Not covered here

- Your own computer while the agent works: see [2 · Protect your environment](2-protect-your-environment.md).

---

Previous: [6 · Reliable code](6-reliable-code.md) · [Index](../README.md) · Next: [8 · Accessible and inclusive products](8-accessible-and-inclusive-products.md)
