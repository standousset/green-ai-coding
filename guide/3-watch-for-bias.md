# 3 · Watch for bias

**Space:** How you choose and use AI
**Principle:** ⚖️ Non-discriminatory · *Who is left out?*

## Why it matters

Code models repeat the biases of their training data. In a study of five models,
13.47% to 49.10% of the generated code had biased behaviour towards gender
([Huang et al.](https://arxiv.org/abs/2309.14345)). Prompt instructions alone
helped little. Feedback from test execution reduced bias much more: for one
model, from 59.88% to 4.79%.

The test data and examples that an assistant generates often show only one
culture: one type of name, one address format, one language.

## What to do

### Everyone

- If your app handles data about people, test it with very different people: long names, non-Latin scripts, other countries.
- Do not let the assistant decide rules about people (access, prices, scores) without a check by a person.

### Developers

- Do not base logic on age, gender, origin or disability unless the task really needs it. Review all logic of this type.
- Add tests that change only one sensitive attribute, such as gender. The result must not change when it should not change.
- Ask for test data with names, scripts, languages and address formats from many cultures. Check generated examples for stereotypes.

### Teams and architects

- List the features that make decisions about people, and give them a specific review.
- Run the tests for these features in the pipeline, so that a change cannot remove them without notice.

## Resources

- [Bias Testing and Mitigation in LLM-based Code Generation](https://arxiv.org/abs/2309.14345):
  the study, and why test feedback works better than prompts.
- [Personal names around the world](https://www.w3.org/International/questions/qa-personal-names)
  (W3C): what to vary in test data for names.

## Not covered here

- Accessibility and inclusion of the product: see [8 · Accessible and inclusive products](8-accessible-and-inclusive-products.md).
- Bias inside the model itself: this is the responsibility of the model provider.

---

Previous: [2 · Protect your environment](2-protect-your-environment.md) · [Index](../README.md) · Next: [4 · Use only the AI you need](4-use-only-the-ai-you-need.md)
