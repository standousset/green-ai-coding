# 8 · Accessible and inclusive products

**Space:** What AI changes in your output
**Principle:** ⚖️ Non-discriminatory · *Who is left out?*

## Why it matters

A 2025 study compared web code from two models with code written by people
([Suh et al.](https://arxiv.org/abs/2503.15885)). The models often did better
on basic points, such as colour contrast and text alternatives for images. They
did worse on complex points, such as ARIA attributes (extra information that tells screen readers what an interface element is and does). Better prompts gave only
small gains. A feedback loop with an accessibility checker gave much better results.

Assistants also tend to repeat defaults from one language or culture: name
fields, date formats, address forms.

## What to do

### Everyone

- Use your app with the keyboard only: can you reach and use everything?
- Run the accessibility check of your browser (for example Lighthouse in Chrome) and ask the assistant to correct the problems that it finds.
- Test forms with names, addresses and dates from other countries.

### Developers

- Run an automatic accessibility checker on generated interfaces, and give the results back to the assistant to correct.
- Also test with a screen reader. Automatic checkers find only part of the problems.

### Teams and architects

- Add automatic accessibility tests to the pipeline, for example axe-core in end-to-end tests.
- Put accessibility and international requirements in the shared rules file and in the review checklist.

## Resources

- [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview): the
  audit tool in Chrome; its accessibility checks use axe-core.
- [Human or LLM? A Comparative Study on Accessible Code Generation Capability](https://arxiv.org/abs/2503.15885):
  the study, and the feedback approach that worked best.
- [axe-core](https://github.com/dequelabs/axe-core): open-source accessibility
  checker that you can run in tests.
- [Personal names around the world](https://www.w3.org/International/questions/qa-personal-names)
  (W3C): how to design name fields for all people.

## Not covered here

- Bias in logic and test data: see [3 · Watch for bias](3-watch-for-bias.md).

---

Previous: [7 · Secure code](7-secure-code.md) · [Index](../README.md) · Next: [9 · Lean code](9-lean-code.md)
