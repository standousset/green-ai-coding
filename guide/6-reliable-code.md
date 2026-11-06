# 6 · Reliable code

**Space:** What AI changes in your output
**Principle:** 🔍 Trustworthy · *Can I check it?*

## Why it matters

AI-made code can call functions that do not exist, use real functions in the
wrong way, or handle only the normal case and forget errors and edge cases. It
can also reproduce code that is under a licence.

You are responsible for this code, the same as for code that you write yourself.

## What to do

### Everyone

- Ask the assistant to write tests, to run them, and to show you the result.
- Try the app with wrong and unusual inputs: empty fields, very long text, wrong formats.

### Developers

- Use the same checks as for human code: tests, type checks (tools that find wrong data types before the code runs) and code review. Do not skip them because the code looks clean.
- Add tests for errors and edge cases (unusual inputs and limits), not only for the normal case.
- If a large block looks copied, find its source. Use your tool's public-code matching filter if it has one.

### Teams and architects

- Make tests and type checks a required step before merge, in the pipeline.
- Scan the project for licences regularly.

## Resources

- [ScanCode Toolkit](https://github.com/aboutcode-org/scancode-toolkit): finds
  licences and copyright notices in code.

## Not covered here

- Security of the code: see [7 · Secure code](7-secure-code.md).
- How you check the assistant while you work: see [1 · Check what AI gives you](1-check-what-ai-gives-you.md).

---

Previous: [5 · Stay in control](5-stay-in-control.md) · [Index](../README.md) · Next: [7 · Secure code](7-secure-code.md)
