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
- Welcome everyone to this presentation on Quality Checks for Developers and Data Professionals.
- My name is John Carlo Santos, and I work primarily as a generalist developer focused on CI/CD and infrastructure.
- Today, we're going to talk about how data professionals can adopt software engineering quality controls—linters, formatters, and tests—to build more resilient pipelines, save time, and onboard team members faster.
-->

---

# John Carlo Santos

### Generalist Developer
* CI/CD & Infra pipelines
* Automation & Guardrails
* Improving developer experience (DX)

<!--
Speaker Notes:
- I currently focus on CI/CD and Infrastructure, and I build tools that help with systems and developers
-->

---

# Why?

- Data Roles(DA, DE, DS) and Software Development boundary is blurring
- High (time) reward action

<!--
Speaker Notes:
- The line between Data Roles are and have been shrinking
- Adding checks to codebase is a high-reward action, yet is easy to implement
-->
---

# Prerequisites

* **Git**: Basic commits, branches, and merges
* **YAML**: Configuration syntax

<!--
Speaker Notes:
- We assume you have a basic grasp of Git (e.g., committing, cloning, pushing).
- YAML is also required because almost every modern developer tool configures its settings using YAML.
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
- Define what quality checks mean in this context. It's about automated, programmatic validation, not just manual verification.
- We divide this into Code Quality (making sure the code is structured correctly, readable, and free of syntax errors) and Data Quality (making sure the data ingested/output matches schemas and limits).
- Highlighting that for data professionals, testing the logic of your transformations is as important as checking the structure of the data.
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
- Give a quick bird-eye view of the tooling landscape.
- In JS/TS, we use Prettier for formatting and ESLint for code smells.
- In Python, which is the staple of data pros, Ruff has replaced Black/Flake8/isort as a blazing fast all-in-one linter/formatter. For testing logic, pytest is standard, while Pandera and Great Expectations validate data frames and schemas.
- In SQL, SQLFluff is a lifesaver for standardizing query formats.
- For shell scripts, ShellCheck prevents common mistakes.
-->

---

<!-- _backgroundColor: #242b35 -->

# Why is this Critical?

<div class="grid-2">
<div>

### ⏱️ Onboarding Savings
* **Unified formatting**: Ends style debates
* **Automated reviews**: Focus on logic
* **One-command setup**: Instant alignment

</div>
<div>

### 🛠️ Maintenance Savings
* **Catch failures locally**: Early feedback
* **Live documentation**: Self-explanatory tests
* **Regression safety**: Confident updates

</div>
</div>

<!--
Speaker Notes:
- Focus on the main productivity gains: onboarding and maintenance.
- For onboarding: standardizing styling with formatters like Black or Ruff means new team members don't need to guess how to style their code. Code reviews are faster because comments aren't cluttered with "add a space here" or "fix indentation".
- For maintenance: tests act as a documentation system. When dependencies change or APIs update, run the test suite to instantly spot failures before they cause pipeline outages.
-->

---

# Why is this Critical? (Data & APIs)

* 🔌 **Fragile APIs**: Mock requests to handle changes
* 💰 **Cost Control**: Prevent expensive query loops
* 🛡️ **Rate Limits**: Run local tests without hitting limits
* 🚯 **Data Trust**: Avoid ingesting garbage data

<!--
Speaker Notes:
- Focus on API-specific and data-specific risks.
- Data professionals write code that fetches from external APIs. APIs change, rate limits get exhausted, and network failures occur. If you do not test these conditions with mock APIs, your production pipelines will crash, or worse, ingest incorrect data.
- Also, point out the financial cost. A bad data pipeline logic error can query terabytes of data repeatedly in a loop, racking up huge costs. Quality checks catch these issues locally.
-->

---

<!-- _backgroundColor: #1e222b -->

# Our Options: The Pipeline

We implement quality checks at different development stages:

<div class="pipeline">
  <div class="step">1. IDE Linting</div>
  <div class="arrow">➔</div>
  <div class="step">2. Git Hooks</div>
  <div class="arrow">➔</div>
  <div class="step">3. CI/CD Run</div>
  <div class="arrow">➔</div>
  <div class="step">4. Production</div>
</div>

<br>
<br>

* **IDE**: Real-time feedback as you type
* **Git Hooks**: Pre-commit blocks bad code locally
* **CI/CD**: Server-side verification before merge

<!--
Speaker Notes:
- Explain the pipeline.
- First, developers get real-time feedback in their editors (IDE extension level).
- Second, we use Git hooks (pre-commit or Husky). This is a script that runs locally whenever a developer types `git commit`. If code is poorly formatted or has syntax errors, it blocks the commit.
- Third, we use CI/CD (GitHub Actions) as the final gatekeeper. If the check passes locally but fails on the server, the pull request cannot be merged.
-->

---

# Local Protection: Pre-Commit

Fast, local git hooks running *before* code leaves your machine

* **Why use it?**: Catch quick style and syntax issues instantly
* **Best for**: Code formatters, trailing whitespace, basic linters
* **Benefit**: Saves CI/CD pipeline queue time and resources

<!--
Speaker Notes:
- Introduce pre-commit hooks. They are git hooks that run locally on your own machine.
- They are extremely fast because they only scan files that you have modified (staged files).
- Ideal for formatting and styling checks where you don't need a heavy server environment.
-->

---

<!-- _backgroundColor: #242b35 -->

# Remote Protection: GitHub Actions

Automated CI/CD workflows running in the cloud

* **Why use it?**: The ultimate gatekeeper before merging to `main`
* **Best for**: Integration tests, database checks, API contract runs
* **Benefit**: Guarantees code runs in a clean, standardized container

<!--
Speaker Notes:
- Introduce GitHub Actions. This is our server-side CI/CD validation.
- Unlike local git hooks which can be bypassed or skipped by a developer, CI/CD runs automatically on GitHub on every pull request.
- It is the ultimate shield protecting your main branch.
-->

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

<!--
Speaker Notes:
- Explain the structure of a GitHub Actions configuration.
- We define when the workflow runs, where it runs (e.g., ubuntu-latest), and the steps it takes to checkout the code and run tests.
-->

---

<!-- _backgroundColor: #1e222b -->

# Pre-Commit vs. CI/CD: Comparison

When to use what?

| Check Type | Pre-Commit (Local) | CI/CD (Remote) |
| :--- | :--- | :--- |
| **Execution** | Local machine | Cloud container |
| **Trigger** | `git commit` (Instant) | Pull Request / Push (1-5 min) |
| **Ideal Tasks** | Formatting, linting, syntax | Integration tests, DB, deploy |
| **Environment** | Developer's local system | Clean, standardized VM |

<!--
Speaker Notes:
- Summarize when to use pre-commit vs CI/CD.
- Pre-commit is for fast, local checks.
- CI/CD is for heavy, server-wide integration testing and deployment.
-->

---

<!-- _backgroundColor: #2c303b -->

# YAML in a Nutshell

Key rules for editing configurations:

<div class="grid-2">
<div>

* **Key-Value**: `key: value` (note the space!)
* **Indentation**: Spaces only (no tabs)
* **Lists**: Prefixed with `-`
* **Multiline**: `|` (literal), `>` (folded)

</div>
<div>

```yaml
# Simple YAML List & Indentation
repos:
  - repo: local
    hooks:
      - id: run-pytest
        name: pytest
        entry: pytest tests/
        language: system
```

</div>
</div>

<!--
Speaker Notes:
- YAML stands for "YAML Ain't Markup Language".
- It is highly human-readable, but strict about spaces.
- Emphasize the importance of indentation and space characters. Explain the hyphen `-` denotes a list item.
- Explain the difference between literal `|` and folded `>` for multiline values, which is super useful for running script commands in GitHub Actions.
-->

---

<!-- _backgroundColor: #1a1d20 -->

# Key Takeaways

* **Pin Down Styling**: Automate formatting first
* **Protect Local**: Catch syntax bugs at commit time
* **Enforce in Cloud**: Run heavier tests in GitHub Actions
* **Deliver Value**: Save onboarding and debugging days

<br>

## Questions & Discussion

<!--
Speaker Notes:
- Summarize the main points.
- Encourage everyone to start small. Don't try to build a massive testing framework on day one. Start by adding a formatter and a simple pre-commit hook.
- Once that is working, add schema validation and mock tests.
- Open the floor for any questions.
-->
