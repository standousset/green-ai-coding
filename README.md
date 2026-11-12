# Green AI coding

Use less energy when you code with Claude. Six levers with sources, and two texts to copy.

This guide is about the energy that the AI uses while it helps you code. It is not about the energy that the code you ship uses when it runs.

> [!NOTE]
> Spot-checked on 12 November 2026 against the Claude docs, the cloud docs and the carbon data. This guide does not measure any saving. Each lever comes from how the models run and how they are billed, and each page says where a claim is an estimate.

## Start here: audit your setup

Paste this into a prompt. The assistant checks your settings and your habits against the levers, and tells you what to change:

```text
Audit how I use you, to reduce energy and tokens. Use the 6 levers below as a checklist. For details, read the README of https://github.com/standousset/green-ai-coding and open a lever page only when you need it. If you cannot open the link, use your own knowledge and say so.
1. Context: how long are my rules or custom instructions (for example CLAUDE.md, AGENTS.md, or a settings field)? Is a rules file longer than 200 lines? Could parts of it move to skills or path-scoped rules? Do my sessions get very long? When does compaction start? Which MCP servers are on?
2. Cache: how do I pay (subscription or API), and so how long does the cache last? Do I change the model or the effort in the middle of a task? Do I take long breaks?
3. Model: is my default model larger than my tasks need?
4. Effort: is my default effort level higher than my tasks need?
5. Round trips: in this conversation, which of my prompts were vague or led to retries? Which tasks could a standard tool do? Do I run agent teams or scheduled tasks?
6. Region: can I choose where my requests run?
Find the settings and rules files of the tool that runs you yourself, at user level and at project level. Read only those files, this conversation, and the list of skills, plugins and connectors that you load. Do not read the whole project and do not change anything. If you cannot read a setting (for example in a desktop app or a chat window), tell me where to find it, and ask me for its value. Be direct. Then give me at most 5 changes, in order of impact: what you found, the change to make, and the reason. Say what you cannot check. End with the list of values you need from me.
```

## Copy this: permanent rules

Paste this into the rules file of your assistant. The assistant then follows it in every session.

```text
Help me use less energy and fewer tokens:
- Be brief. Show only the code that changes. No summary at the end unless I ask.
- Read only the files that the task needs. Ask before you read the whole project.
- Keep long outputs (tests, logs, searches) small: filter them at the source, and show me only the result.
- Make the smallest change that works. No extra features, files or dependencies.
- If a standard tool can do the task without AI (search, rename, formatter, linter), tell me.
- If my request is not clear, ask all the questions needed before starting.
```

Claude Code reads `AGENTS.md` only when no `CLAUDE.md` exists. If you have a `CLAUDE.md`, write `@AGENTS.md` in it ([Claude Code docs](https://code.claude.com/docs/en/memory#agents-md)).

A rules file acts on levers 1 and 5. The other levers are settings: use the audit above for them.

Optional, and a different topic: if you also want the code that you ship to be light to run, add this line. This guide does not cover that topic.

```text
- Write code that is light to run: avoid needless loops, queries, network calls and large data.
```

## The 6 levers

Levers 1 to 5 apply to everyone. Lever 6 applies only if you can choose where your requests run.

1. **[Keep the context short](levers/lever-1-keep-the-context-short.md)**: each request sends the whole conversation again. Run `/clear` between tasks, send only what the task needs, give noisy work to a subagent, and compact a long task.
2. **[Stay in the cache window](levers/lever-2-stay-in-the-cache-window.md)**: the provider reuses its work on the start of the conversation for 5 minutes or 1 hour. Choose the model and the effort before the task, do not change them in the middle, and compact before a long break.
3. **[Use a smaller model](levers/lever-3-use-a-smaller-model.md)**: a larger model uses more energy for each token. Make Sonnet your default, and use Opus for the hardest tasks only.
4. **[Lower the effort](levers/lever-4-lower-the-effort.md)**: thinking is text that the model writes, and each token uses energy. Use a low or medium level for most tasks, and go up only when needed.
5. **[Make fewer round trips](levers/lever-5-make-fewer-round-trips.md)**: each retry sends the whole conversation again, with the failed reply. Name the file, plan a large task first, run `/clear` after two failed corrections, and stop agents that run without you.
6. **[Choose a low-carbon region](levers/lever-6-choose-a-low-carbon-region.md)**: in 2025, electricity emitted 28 gCO₂/kWh in Norway and 670 in India. Use this on Amazon Bedrock or Google Cloud, where you can choose the region.

Each page has short items with the same parts: what to do, why, how, and where (terminal, desktop app or chat).

## Limits

- The pages cover Claude (Claude Code, the desktop app and the chat). Other assistants have similar settings under other names.
- Levers 1 and 2 follow from billing and from reliability more than from energy. In a test of local models, the output length and the model size explained most of the energy, and the length of the prompt explained little ([Alizadeh et al.](https://arxiv.org/abs/2412.00329)). Levers 3 and 4 act on those two factors.
- Lever 3 relies on an estimate of the energy for each token, because the size of the Claude models is not public.
- Agents that run alone and MCP servers have one short item each (5.9 and 1.2).

## Feedback

Something wrong, out of date or missing? [Open an issue](https://github.com/standousset/green-ai-coding/issues).

## License

[MIT](LICENSE)
