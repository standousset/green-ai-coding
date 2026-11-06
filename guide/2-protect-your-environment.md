# 2 · Protect your environment

**Space:** How you choose and use AI
**Principle:** 🔒 Secure · *Can it be attacked?*

## Why it matters

An assistant works on your computer, with your files and your access. This
creates new ways to attack you:

- **Invented packages.** In a study of 16 code models and 576,000 code samples,
  at least 5.2% of the packages that commercial models suggested did not exist,
  and 21.7% for open-source models
  ([Spracklen et al., 2024](https://arxiv.org/abs/2406.10279)). Attackers can
  publish malicious packages under these names ("slopsquatting").
- **Prompt injection.** Text that the agent reads, for example in a web page, an
  issue or a file, can contain hidden instructions that change what the agent
  does ([OWASP](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)).
- **Too much access.** An agent with too many permissions can leak secrets or
  cause damage after one wrong output
  ([OWASP](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)).

Ignore files and permission rules help, but they are not always a security
boundary. For example, the Claude Code documentation says that its command
rules are not a security boundary, and recommends its sandbox (an isolated space that limits which files and network addresses commands can reach) for that purpose.

## What to do

### Everyone

- Never put real passwords, API keys or tokens in the project or in a prompt. Keep them outside the project folder, for example in a secrets manager.
- Keep the agent in a mode where it asks before each command, unless you understand what it runs.
- Before you install a package that the assistant suggests, find it on the official package registry and check its age and number of downloads.

### Developers

- Treat everything the agent reads from outside as untrusted. Check the commands that it proposes after it reads web pages, issues or third-party files.
- Run the agent in a sandbox or a container, and block it from reading secret files.
- Read your provider's terms: where your code is stored, how long, and if it is used to train models.

### Teams and architects

- Choose the providers and their settings for the whole team: data retention, use for training, location of the data.
- Set assistant permissions and exclusions at organisation level, where your tools allow it.
- Add a package check to the pipeline: stop new dependencies that do not exist, that are very new, or that the team did not approve.

## Resources

- [Prompts for this part](../toolkit/prompts.md#2--protect-your-environment): ready to copy into your assistant.
- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/): the
  main risks, including prompt injection, data disclosure and excessive agency.

  > **In short** (checked 2026-11-06): the ten risks of the 2025 list are prompt injection, sensitive
  > information disclosure, supply chain, data and model poisoning, improper output handling, excessive
  > agency, system prompt leakage, vector and embedding weaknesses, misinformation, and unbounded
  > consumption. In our view, five apply directly to coding assistants: prompt injection, information
  > disclosure, supply chain, excessive agency and misinformation.

- [We Have a Package for You!](https://arxiv.org/abs/2406.10279): the study of
  invented packages.
- Security settings for common assistants:
  - Claude Code: [security](https://code.claude.com/docs/en/security),
    [permissions](https://code.claude.com/docs/en/permissions) and
    [sandbox](https://code.claude.com/docs/en/sandboxing)
  - Cursor: [ignore file](https://cursor.com/docs/reference/ignore-file)
  - GitHub Copilot: [content exclusion](https://docs.github.com/en/copilot/how-tos/configure-content-exclusion/exclude-content-from-copilot)

## Not covered here

- Security of the code that you ship: see [7 · Secure code](7-secure-code.md).
- Decisions about actions that you cannot undo: see [5 · Stay in control](5-stay-in-control.md).

---

Previous: [1 · Check what AI gives you](1-check-what-ai-gives-you.md) · [Index](../README.md) · Next: [3 · Watch for bias](3-watch-for-bias.md)
