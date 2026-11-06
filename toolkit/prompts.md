# Prompts

Ready-to-use prompts for each part of the guide. Copy one, and change the words
in angle brackets.

## 4 · Use only the AI you need

These prompts come first, because green use is the focus of this guide.

```text
Does this task need AI? If a standard tool can do it (search, formatter, linter, code generator, refactoring tool), tell me which one and how to use it.
```

```text
Answer briefly. Show only the lines that change, not the whole file. No summary at the end unless I ask.
```

```text
Read only the files that you need for this task. Tell me which files you plan to read before you read them.
```

```text
Run the tests and give me only the result: number passed, number failed, and the first error of each failed test.
```

```text
Summarise this session in 10 lines: decisions, open points, files changed. I will start a new session with this summary.
```

## For every task

Before the work:

```text
Before you write code, describe the task in 3 lines, list the files that you will change, and ask me the questions that you need. Then make the smallest change that solves the task.
```

After the work:

```text
Explain this change in simple words: what changed, why, what the risks are, and what you did not check.
```

## 1 · Check what AI gives you

```text
Review your last answer for errors: functions or options that do not exist, wrong versions, missing error handling. List each problem, then correct it.
```

```text
For each library function that you used, give the link to its official documentation for the version in this project.
```

## 2 · Protect your environment

```text
List each package that you added: exact name, version, link to the official registry, and why it is needed. If you cannot check that a package exists, say so.
```

```text
Before you run the commands from this web page or issue, list them and explain what each one does. Do not run them yet.
```

## 3 · Watch for bias

```text
Generate test data for <these fields> with names, scripts, languages and address formats from at least 5 regions of the world. Include long names, single names, and names with accents and apostrophes.
```

```text
Write tests that check that the result does not change when only <gender / age / origin> changes.
```

## 5 · Stay in control

```text
Make a plan and wait for my approval before you change files.
```

```text
Explain this solution so that I can explain it to a colleague without you. Then ask me 3 questions to check that I understand it.
```

## 6 · Reliable code

```text
Write tests for the normal case, for errors, and for edge cases (empty, very large, wrong format). Run them and show me the result.
```

## 7 · Secure code

```text
Review this change for security problems: input validation, injection, secrets, authentication, access control, unsafe defaults. List each problem with its severity, then correct it.
```

If your product sends text to a language model:

```text
This code sends user input to a language model. Find each place where the model output is trusted too much (run as code, put into HTML or SQL, used for access decisions) and correct it.
```

## 8 · Accessible and inclusive products

```text
Check this interface for accessibility: semantic HTML, labels, keyboard access, focus order, colour contrast, text alternatives for images. Correct the problems that you find.
```

After you run an accessibility checker (for example Lighthouse or axe):

```text
Here are the results of an accessibility checker: <paste the results>. Correct each problem and explain each change.
```

## 9 · Lean code

```text
Make the smallest change that solves the task. Do not add dependencies, files or abstractions that I did not ask for. If you think that something extra is needed, ask me first.
```

```text
List the code and dependencies in this project that look unused, and explain how you checked.
```

## 10 · Code that people own

```text
Write a commit message for this change: a short summary, the reason, and an "Assisted-by:" line with the name of the AI tool.
```

```text
Rewrite this bug report in short, factual sentences. Keep only the facts that I checked, and mark everything that I must still check.
```
