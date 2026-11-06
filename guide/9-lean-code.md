# 9 · Lean code

**Space:** What AI changes in your output
**Principle:** 🌱 Eco-friendly · *Is it necessary?*

## Why it matters

With AI, code is cheap to write. So it is easy to add code, dependencies and
features that nobody needs. Each of them still costs something: maintenance,
download size, energy on users' devices and servers, and more places for
security faults.

If your product calls an AI model, each call also has an energy cost, for each
user, for the life of the product.

## What to do

### Everyone

- Ask the assistant for the smallest change that solves the task. Refuse extras that you did not ask for.
- Before you ask AI to build a feature, ask if the feature is needed.

### Developers

- Check each new dependency: is it necessary, or can a few lines of code do the same?
- Remove unused code and dependencies regularly.

### Teams and architects

- Run checks for unused code and dependencies in the pipeline.
- If your product calls an AI model, measure the impact of those calls, use the smallest model that is good enough, and keep answers that you can use again.

## Resources

- [Prompts for this part](../toolkit/prompts.md#9--lean-code): ready to copy into your assistant.
- [Knip](https://knip.dev/): finds unused files, exports and dependencies in
  JavaScript and TypeScript projects.
- [deptry](https://deptry.com/): finds unused, missing and indirect
  dependencies in Python projects.
- [EcoLogits](https://ecologits.ai/): estimates the impact of the AI calls that
  your product makes.

## Not covered here

- The energy of your own use of AI while you code: see [4 · Use only the AI you need](4-use-only-the-ai-you-need.md).

---

Previous: [8 · Accessible and inclusive products](8-accessible-and-inclusive-products.md) · [Index](../README.md) · Next: [10 · Code that people own](10-code-that-people-own.md)
