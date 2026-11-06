# responsible-ai-coding

Tools for developers who code with AI and want to do it in a responsible way.

The tools cover two spaces, and four principles apply to both.

```mermaid
---
config:
  flowchart:
    wrappingWidth: 400
---
flowchart LR
  accTitle: Two spaces, four principles
  accDescr: For each of the four principles, what to do when you choose and use AI, and the typical faults that AI brings into your output.

  PT["<b>Trustworthy</b><br/><i>Can I check it?</i>"]
  PF["<b>Non-discriminatory</b><br/><i>Who is left out?</i>"]
  PE["<b>Eco-friendly</b><br/><i>Is it necessary?</i>"]
  PH["<b>At the service of people</b><br/><i>Who decides?</i>"]

  subgraph S1["1 · Choose and use AI"]
    T1["Know where your code and data go<br/>Check the code and packages it adds<br/>Keep secrets out of your prompts"]
    F1["Check generated test data<br/>for stereotypes"]
    E1["Choose a model of the right size<br/>for the task"]
    H1["Keep personal data out of prompts<br/>Limit what an agent can do<br/>Decide yourself on risky actions"]
  end

  subgraph S2["2 · Faults in AI output"]
    T2["Invented functions and APIs<br/>Insecure defaults<br/>Code copied under a licence"]
    F2["Interfaces that are not accessible<br/>Defaults for one language or culture"]
    E2["Code and dependencies not needed<br/>Features made because they are cheap"]
    H2["Code that nobody can explain<br/>No record of where AI was used"]
  end

  PT ~~~ T1 ~~~ T2
  PF ~~~ F1 ~~~ F2
  PE ~~~ E1 ~~~ E2
  PH ~~~ H1 ~~~ H2

  classDef trust stroke:#2f81f7,stroke-width:3px
  classDef fair stroke:#a371f7,stroke-width:3px
  classDef eco stroke:#2da44e,stroke-width:3px
  classDef human stroke:#e16f24,stroke-width:3px
  class PT,T1,T2 trust
  class PF,F1,F2 fair
  class PE,E1,E2 eco
  class PH,H1,H2 human
```

## 1. How you choose and use AI

Choose your AI coding assistant, and use it, in a responsible way:

- know where your code and data go, and choose a provider that you can trust;
- choose a model of the right size for the task;
- keep secrets and personal data out of your prompts;
- limit what an agent can do: commands, files, network access;
- check what the assistant writes before you accept it, including the packages it adds;
- check that generated examples and test data have no stereotypes;
- keep the final decision for risky actions.

## 2. What AI changes in your output

Code made with AI has its own typical faults. Find them and correct them:

- invented functions, insecure defaults, code copied under a licence;
- interfaces that are not accessible, and defaults that assume one language or one culture;
- code and dependencies that the task does not need, and features made only because AI makes them cheap;
- code that nobody on the team can explain, and no record of where AI was used.

## Principles

Both spaces follow the same four principles:

- **Trustworthy**
- **Non-discriminatory**
- **Eco-friendly**
- **At the service of people**

> No tool is published yet.

## Licence

[MIT](LICENSE)
