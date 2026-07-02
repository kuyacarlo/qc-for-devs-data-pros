# JS/TS Quality Checks (Pre-Commit & GitHub Actions)

This folder contains pre-commit and CI configurations for JavaScript & TypeScript environments.

## 📁 Included Files
* **`.pre-commit-config.yml`**: Config for local hook validation.
* **`.github/workflows/ci.yml`**: Config for remote GitHub Actions pipeline.

---

## ⚡ 1. Local Git Hooks (`pre-commit`)

### Hooks
* **Standard checks**: Whitespace, EOF, JSON formatting, and large files.
* **`prettier`**: Formats Javascript, TypeScript, CSS, HTML, JSON, and Markdown.
* **`eslint`**: Lints Javascript and TypeScript code.
* **`gitleaks`**: Scans for secrets before committing.

> [!NOTE]
> For JS/TS projects, you can alternatively use **Husky** and **lint-staged** to keep all config and scripts within your `package.json` and node modules ecosystem. If you choose to do that, you don't need `pre-commit` installed.

### Local Setup
1. Copy configuration to repository root:
   ```bash
   cp .pre-commit-config.yml ../../.pre-commit-config.yml
   ```
2. Install git hooks:
   ```bash
   pre-commit install
   ```
3. Test by running:
   ```bash
   pre-commit run --all-files
   ```

---

## ☁️ 2. Remote Pipeline (GitHub Actions)

### Jobs
* **Dependency Installation**: Runs `npm ci` to get a reproducible dependency tree.
* **Lint and Format**: Validates lint standards and formatting checks if defined in `package.json` scripts.
* **Strict Jest Test Suite**: Executes tests while dumping results into a JSON report. A small inline Node script reads this report and fails the build if any test case is marked as pending, skipped, or todo. This prevents faked tests or test-skipping by AI or developers.

### Setup
1. Copy the workflow file to your project:
   ```bash
   mkdir -p ../../.github/workflows/
   cp .github/workflows/ci.yml ../../.github/workflows/ci.yml
   ```
2. Adjust package settings (e.g. `pnpm` or `yarn` caching/installation commands if not using npm).
