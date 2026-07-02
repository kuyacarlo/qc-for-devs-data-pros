---
marp: true
title: Quality Checks for Developers
description: Learn how you can continuously check for code quality as you build, push and deploy your code.
author: John Carlo Santos
backgroundColor: #212529
color: #F2D9D0
style: |
  section {
    font-family: 'Outfit', 'Inter', sans-serif;
    padding: 50px;
    font-size: 24px;
    background-color: #212529;
    color: #F2D9D0;
  }
  h1 {
    color: #FFB38A;
    font-size: 2em;
    margin-top: 0px !important;
    margin-bottom: 25px !important;
    padding-top: 0px !important;
    line-height: 1.2;
  }
  h2 {
    color: #FFD2B3;
    font-size: 1.4em;
    margin-top: 0px !important;
    margin-bottom: 20px !important;
  }
  h3 {
    color: #FFB38A;
    font-size: 1.1em;
    margin-top: 0px !important;
    margin-bottom: 10px !important;
  }
  footer {
    font-size: 0.6em;
    color: rgba(242, 217, 208, 0.5);
  }
  a {
    color: #4DD0E1;
    text-decoration: none;
  }
  code {
    background-color: #343a40;
    color: #4DD0E1;
    border-radius: 4px;
    padding: 2px 6px;
    font-size: 0.85em;
  }
  .grid-2 {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
  }
  .highlight {
    color: #FFB38A;
    font-weight: bold;
  }
  .tag {
    color: #4DD0E1;
    font-size: 0.8em;
    display: inline-block;
    margin-bottom: 1px;
  }
  table {
    width: 100% !important;
    border-collapse: collapse;
    margin-top: 15px;
    background-color: transparent !important;
  }
  tr, th, td {
    background-color: transparent !important;
    color: #F2D9D0 !important;
  }
  th {
    color: #FFD2B3 !important;
    border-bottom: 2px solid #FFB38A !important;
    padding: 10px 12px;
    text-align: left;
  }
  td {
    padding: 10px 12px;
    text-align: left;
  }
  .pipeline {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-top: 40px;
    width: 100%;
  }
  .step {
    background-color: #343a40;
    border: 2px solid #FFB38A;
    padding: 15px;
    border-radius: 8px;
    text-align: center;
    font-weight: bold;
    font-size: 0.85em;
    color: #F2D9D0;
    flex-grow: 1;
    margin: 0 10px;
  }
  .step.flag {
    border-color: #4DD0E1;
  }
  .arrow {
    color: #4DD0E1;
    font-size: 1.4em;
    font-weight: bold;
  }
---

<!-- _backgroundColor: #1a1d20 -->

# <p class="tag"> Quality Checks for Developers & Data Pros </p>

Check quality as you write, commit, and deploy code.

<br>

**John Carlo Santos**
*Generalist Developer (Focusing on CI/CD & Infra)*

<!--
Speaker Notes:
- Welcome. Keep bio intentionally light here — the ComplyAIgent reveal later does the "credibility" work, don't spend it now.
-->

---

# Tonight

1. **Poll** — quick gut-check on where the room's at
2. **Why** — the blurring line between DA / DE / DS / DG
3. **What** — quality checks, secrets scanning, schema validation
4. **How** — the pipeline, a live tool reveal, and building it together
5. **Build it** — live coding on a real repo

<!--
Speaker Notes:
- Agenda slide, keep it fast. Sets expectation this is lecture-then-build, not pure lecture.
-->

---

# Quick Poll

Drop a reaction in Discord:

* 🟦 Used **YAML** before?
* 🟩 Used **GitHub Actions** / CI-CD?
* 🟨 Used **pre-commit** hooks?
* 🟥 Ever built a code quality flow from scratch?

<!--
Speaker Notes:
- Platform is Discord — run as a reaction poll (or poll bot) at the start, not a show of hands. Give it ~30-60s to land before checking counts.
- Use the results to calibrate how much to explain vs. skip later — if reactions are light on pre-commit, slow down there.
-->

---

# Prerequisites

* **Git**: Basic commits, branches, and merges
* **YAML**: Configuration syntax

<!--
Speaker Notes:
- Brief. Templates handle the depth — this is just a level-set.
-->

---

<!-- _backgroundColor: #242b35 -->

# Why?

* The line between **DA, DE, DS, DG** is blurring
* Poll probably just proved it
* Quality checks are a **high-reward, low-effort** habit

<!--
Speaker Notes:
- Let the poll results do most of the talking here. This should be quick.
-->

---

<!-- _backgroundColor: #2c303b -->

# What are "Quality Checks"?

Programmatic code validation before deployment

<div class="grid-2" style="margin-top: 20px;">
<div>

### Code Quality
* **Formatting**: Consistent styling
* **Linting**: Static analysis bugs
* **Testing**: Functional correctness

</div>
<div>

### Data Quality
* **Schema**: Columns & types
* **Pipeline**: Input validation
* **Mocking**: Safe API testing

</div>
</div>

<!--
Speaker Notes:
- Code Quality = correctness/readability of code itself. Data Quality = correctness of what flows through it.
-->

---

<!-- _backgroundColor: #242b35 -->

# What Are We Protecting?

* **Secrets & Credentials**: API keys, database URIs, token strings. (Once committed, they are permanently compromised!)
* **Code Integrity**: Broken dependencies, invalid syntax, or missing configs.
* **Data Schemas**: Bad fields, structural type drift, or empty outputs.
* **Team Velocity**: Automation stops bikeshedding on format style during PRs.

<!--
Speaker Notes:
- Secrets: Once written to a git commit, it is in your git history forever. Deleting it in a later commit is NOT enough.
- Code/Data integrity: Block compile/import errors from leaving local environments.
- Velocity: Formatting tools take human bias out of code reviews.
-->

---

# Secrets Scanning: Fast vs. Thorough

<div class="grid-2">
<div>

### ⚡ Local (Pre-Commit)
* Scans **staged files only**
* Seconds
* Catches the obvious: hardcoded keys, `.env` in a commit

</div>
<div>

### 🔍 Remote (CI)
* Scans **full repo / history**
* Minutes
* Catches the sneaky: obfuscated, split, or encoded secrets

</div>
</div>

<!--
Speaker Notes:
- This is the "when to use which" metric: speed vs. coverage. Fast local check for the obvious stuff, slower CI-stage check for what regex/humans miss.
- This sets up the tool reveal later — don't name it yet.
-->

---

# Schema Validation, in a Nutshell

* Checks the **shape** of data, not the data itself
* No real dataset needed — validate against a dummy/mock output
* The check **travels with the repo**; the data doesn't
* (Pandera — Python-native, no extra config to learn tonight)

<!--
Speaker Notes:
- Key teaching point: you don't need real/sensitive data in the repo to validate structure. One-liner, don't deep-dive — templates will have an example.
-->

---

# Tooling Across Languages

<table>
  <thead>
    <tr>
      <th>Language</th>
      <th>Lint & Format</th>
      <th>Test & Validate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Python</strong></td>
      <td>Ruff, Black</td>
      <td>pytest, Pandera</td>
    </tr>
    <tr>
      <td><strong>SQL</strong></td>
      <td>SQLFluff</td>
      <td>dbt tests</td>
    </tr>
    <tr>
      <td><strong>JS / TS</strong></td>
      <td>Prettier, ESLint</td>
      <td>Jest, Husky</td>
    </tr>
    <tr>
      <td><strong>Shell</strong></td>
      <td>ShellCheck</td>
      <td>Bats</td>
    </tr>
  </tbody>
</table>

<!--
Speaker Notes:
- Mention only, don't dwell. This is on the resources slide/repo too, so no need to read it line by line.
-->

---

<!-- _backgroundColor: #1e222b -->

# The Pipeline

<div class="pipeline">
  <div class="step">IDE Linting</div>
  <div class="arrow">➔</div>
  <div class="step flag">Pre-Commit<br><span style="font-size:0.75em;font-weight:normal;">secrets (fast) + formatting</span></div>
  <div class="arrow">➔</div>
  <div class="step flag">CI/CD<br><span style="font-size:0.75em;font-weight:normal;">secrets (deep) + schema checks</span></div>
  <div class="arrow">➔</div>
  <div class="step">Production</div>
</div>

<!--
Speaker Notes:
- Same 4-stage pipeline as before, now annotated with where secrets scanning and schema validation actually sit. This is the visual anchor for the whole talk.
-->

---

<!-- _backgroundColor: #242b35 -->

# The Cost of Lax Enforcement

* **The Incident**: An automated/AI generation tool faked test success by silently skipping tests.
* **The Root Cause**: Main branch allowed merging without verifying that tests actually ran and passed.
* **The Damage**: Half the team (2 out of 4) burned an entire sprint fixing production bugs that a single CI rule would have blocked.

<!--
Speaker Notes:
- The Incident: Automated code gen makes it easy to ship features, but also extremely easy to ship bugs at scale if you don't enforce checks. Here, tests were bypassed/skipped, and it merged because there was no active block.
- Root Cause: Trusting "tests exist" or "build compiled" is not enough. We must enforce that tests actually execute successfully.
- Damage: 50% of the engineering team's capacity for a sprint was lost cleaning up production errors. A 10-minute CI configuration update would have caught this instantly.
-->

---

<!-- _backgroundColor: #1a1d20 -->

# The Reveal

*(live demo — no slide content, just the terminal)*

* nutrition-api's `.testenv`, with two planted secrets: one obvious, one obfuscated
* Pre-commit catches the obvious one instantly
* The obfuscated one slips past regex — caught in CI instead

<!--
Speaker Notes:
- This is the ComplyAIgent moment. Show terminal output / API response, not the frontend (it's a stub).
- Uses the same repo/.testenv as the live-coding block later — plant the secrets here ahead of time, then it's already in place when you get to "Let's Build It."
- Don't over-narrate the tool name/backstory — let the demo make the point, this is the "surprise" beat.
-->

---

# Local Protection: Pre-Commit

Fast, local git hooks running *before* code leaves your machine

* **Best for**: Formatters, trailing whitespace, basic linters, fast secret scans
* **Benefit**: Saves CI/CD queue time and resources

---

<!-- _backgroundColor: #242b35 -->

# Remote Protection: GitHub Actions

Automated CI/CD workflows running in the cloud

* **Best for**: Integration tests, schema/data checks, deep secret scans
* **Benefit**: Clean, standardized environment — can't be skipped locally

---

# GitHub Actions: General Format

Workflows configured in `.github/workflows/ci.yml`

<div class="grid-2">
<div>

* **name**: Workflow identifier
* **on**: Triggers (push, pull_request)
* **jobs**: List of parallel tasks
* **steps**: Sequential shell commands

</div>
<div>

```yaml
name: CI
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Tests
        run: |
          pip install ruff pytest
          ruff check .
          pytest
```

</div>
</div>

---

<!-- _backgroundColor: #1e222b -->

# Let's Build It

We'll set up a quality-check flow on **[nutrition-api](https://github.com/kuyacarlo/nutrition-api)** together:

* FastAPI server (`server/`), migrations (`alembic/`), `test/` suite
* Already has a `.testenv` — we'll use it as our secrets-scanning target
* Already has `.github/workflows` — we'll extend it, not start from zero

Steps: pre-commit config → plant + catch a secret in `.testenv` → extend the existing CI workflow to fail on skipped tests

<!--
Speaker Notes:
- Transition slide into live coding. Templates/starter files provided separately — don't make people type boilerplate live.
- Repo already has a .testenv file and an existing GitHub Actions workflow — use both as real anchors instead of building fake ones. Extending real config lands better than a toy example.
- Ties directly back to the failure story: the CI check we add here is the "fail if tests are skipped" check that would've caught the AI-generated code problem.
-->

---

<!-- _backgroundColor: #1a1d20 -->

# Key Takeaways

* **Pin Down Styling**: Automate formatting first.
* **Protect Local**: Fast checks catch the obvious — secrets, syntax.
* **Enforce in Cloud**: Slower, deeper checks — schema, deep secret scans.
* **Adopt Incrementally**: Start with standard formatting/linters, add secrets scanning, then enforce test rules.
* **Deliver Value**: Save onboarding and debugging days.

---

# Resources

* Starter repo: `[link/QR]`
* `.pre-commit-config.yaml` template
* `ci.yml` template
* Tooling reference table

<br>

## Questions & Discussion

<!--
Speaker Notes:
- QR code goes here. This is what people actually take home.
-->
