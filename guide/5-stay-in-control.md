# 5 · Stay in control

**Space:** How you choose and use AI
**Principle:** 👥 At the service of people · *Who decides?*

## Why it matters

The assistant proposes; a person decides. Agents can now do many steps without
a person: run commands, change many files, open pull requests. Each step that
nobody checks is a decision that nobody made.

Prompts can also contain personal data of real people, who did not agree to
send it to an AI provider.

## What to do

### Everyone

- Keep approval on for actions that you cannot undo: delete, publish, deploy, send.
- Never put personal data of real people in a prompt. Use fake data.
- Do not ship what you do not understand: ask the assistant to explain until you can explain it yourself.

### Developers

- A person reviews and accepts each change before merge. The assistant can help with the review, but it does not make the final decision.
- Keep your skills: solve some problems without AI, above all when you learn something new.

### Teams and architects

- Decide which actions agents can do alone and which need a person, for example no direct push to the main branch and no deployment to production. Make the tools apply this rule (for example with protected branches).
- Write a short team policy for AI use: which tools, which data, which review rules.

## Resources

- [Prompts for this part](../toolkit/prompts.md#5--stay-in-control): ready to copy into your assistant.
- [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
  (GitHub): make the platform require a review before merge.
- [Fedora AI-Assisted Contributions Policy](https://docs.fedoraproject.org/en-US/council/policy/ai-contribution-policy/):
  AI can help reviewers, but a person makes the final decision.
- [Claude Code permissions](https://code.claude.com/docs/en/permissions): an
  example of rules that make an agent ask before it acts.

## Not covered here

- Permissions as a security measure: see [2 · Protect your environment](2-protect-your-environment.md).
- Who owns the code after the merge: see [10 · Code that people own](10-code-that-people-own.md).

---

Previous: [4 · Use only the AI you need](4-use-only-the-ai-you-need.md) · [Index](../README.md) · Next: [6 · Reliable code](6-reliable-code.md)
