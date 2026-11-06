# 10 · Code that people own

**Space:** What AI changes in your output
**Principle:** 👥 At the service of people · *Who decides?*

## Why it matters

A person is responsible for the code, not the tool. The Fedora project says it
clearly in its policy: the contributor is the author and is accountable for the
whole contribution
([Fedora AI-Assisted Contributions Policy](https://docs.fedoraproject.org/en-US/council/policy/ai-contribution-policy/)).

Code that nobody on the team understands cannot be maintained or corrected. AI
output also has a cost for other people: open-source maintainers such as the
curl project now ask contributors not to send long AI-generated texts, and to
explain issues in their own words
([curl](https://curl.se/dev/vuln-disclosure.html)).

## What to do

### Everyone

- Ship only code that you, or a person on your team, can explain.
- When you report a bug or contribute to another project: write in your own words, check every fact, and follow the AI policy of the project.

### Developers

- Record where AI helped. For example, add an `Assisted-by:` line at the end of the commit message, as the Fedora policy recommends.
- Send small changes that you can explain, not large AI-made changes that nobody asked for.

### Teams and architects

- Decide how the team records AI use (commit line, pull request field), and write it down.
- Make sure that each part of the system has a person who understands it and is responsible for it (for example with a code owners file).

## Resources

- [About code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
  (GitHub): name the people responsible for each part of the code.
- [Fedora AI-Assisted Contributions Policy](https://docs.fedoraproject.org/en-US/council/policy/ai-contribution-policy/):
  a clear example of a project policy, with the `Assisted-by:` trailer.
- [curl vulnerability disclosure policy](https://curl.se/dev/vuln-disclosure.html):
  what a maintainer expects from reports in the age of AI.

## Not covered here

- Decisions while the agent works: see [5 · Stay in control](5-stay-in-control.md).

---

Previous: [9 · Lean code](9-lean-code.md) · [Index](../README.md)
