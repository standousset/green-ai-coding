# responsible-ai-coding

How to code with AI in a green way, from the occasional vibe-coder to the
architect.

## The levers that make the difference

Each request to an AI assistant uses energy. These choices change it the most:

1. **Lower the effort for simple tasks.** Thinking can add tens of thousands of
   tokens to one request, and reasoning models use on average **30× more
   energy**. [→ Lever 1](guide/4-use-only-the-ai-you-need.md#1-lower-the-effort-for-simple-tasks)
2. **Use the smallest model that does the task.** The energy for each generated
   token grows with the size of the model.
   [→ Lever 2](guide/4-use-only-the-ai-you-need.md#2-use-the-smallest-model-that-does-the-task)
3. **Keep the context small.** The full conversation is sent again with
   **every** request. Start a new session for each task.
   [→ Lever 3](guide/4-use-only-the-ai-you-need.md#3-keep-the-context-small)
4. **Stay inside the cache window.** A cache read costs about **0.1×**, but the
   cache lasts **5 minutes** by default and each model has its own cache. Change
   the model between tasks, not in the middle of one.
   [→ Lever 4](guide/4-use-only-the-ai-you-need.md#4-stay-inside-the-cache-window)
5. **Turn off the tools that you do not use.** Each connected tool server adds to
   the context. [→ Lever 5](guide/4-use-only-the-ai-you-need.md#5-turn-off-the-tools-that-you-do-not-use)
6. **Send noisy work to a small subagent.** Tests, logs and searches stay out of
   the main context. [→ Lever 6](guide/4-use-only-the-ai-you-need.md#6-send-noisy-work-to-a-small-subagent)
7. **Ask precisely, and plan first.** Each failed attempt is a full extra round.
   [→ Lever 7](guide/4-use-only-the-ai-you-need.md#7-ask-precisely-and-plan-first)
8. **Do not use AI when a tool does the job.** Search, rename, format.
   [→ Lever 8](guide/4-use-only-the-ai-you-need.md#8-do-not-use-ai-when-a-tool-does-the-job)

When you can choose where requests run: electricity emits from **28 gCO₂/kWh**
(Norway) to **670** (India), a gap of more than **20×**.
[→ Low-carbon region](guide/4-use-only-the-ai-you-need.md#when-you-can-choose-a-low-carbon-region)

All numbers and their sources: [4 · Use only the AI you need](guide/4-use-only-the-ai-you-need.md).

## Quick start

The levers above are choices that you make. This text is for the assistant:
copy it into your prompt, your `AGENTS.md`, or your assistant's settings.

```text
Work with me in a green and responsible way:
- 🌱 Be brief: use the fewest words that are clear, and show only the code that changes.
- 🌱 Read only the files that the task needs. Summarise long outputs (tests, logs) instead of repeating them.
- 🌱 Make the smallest change that works, with no extra features, files or dependencies.
- 🌱 If a simple tool or command can do the task without AI, tell me.
- 🔍 🔒 Do not invent functions or packages, keep secrets out of code, and ask before actions that I cannot undo.
- 👥 Explain each change in plain words, with its risks and what you did not check.
```

More: the [full rules block](toolkit/rules.md) for your `AGENTS.md` file, and
[ready-to-use prompts](toolkit/prompts.md).

## Other principles

Green use is the focus of this guide. The other principles also matter when you
code with AI:

| Principle | How you choose and use AI | What AI changes in your output |
|---|---|---|
| 🔍 **Trustworthy**<br>*Can I check it?* | [1 · Check what AI gives you](guide/1-check-what-ai-gives-you.md) | [6 · Reliable code](guide/6-reliable-code.md) |
| 🔒 **Secure**<br>*Can it be attacked?* | [2 · Protect your environment](guide/2-protect-your-environment.md) | [7 · Secure code](guide/7-secure-code.md) |
| ⚖️ **Non-discriminatory**<br>*Who is left out?* | [3 · Watch for bias](guide/3-watch-for-bias.md) | [8 · Accessible and inclusive products](guide/8-accessible-and-inclusive-products.md) |
| 🌱 **Eco-friendly**<br>*Is it necessary?* | **[4 · Use only the AI you need](guide/4-use-only-the-ai-you-need.md)** | [9 · Lean code](guide/9-lean-code.md) |
| 👥 **At the service of people**<br>*Who decides?* | [5 · Stay in control](guide/5-stay-in-control.md) | [10 · Code that people own](guide/10-code-that-people-own.md) |

## Licence

[MIT](LICENSE)
