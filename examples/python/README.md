# Python Quality Checks (Pre-Commit & GitHub Actions)

This folder contains pre-commit and CI setups optimized for Python environments.

## 📁 Included Files
* **`.pre-commit-config.yml`**: Config for local hook validation.
* **`.github/workflows/ci.yml`**: Config for remote GitHub Actions pipeline.

---

## ⚡ 1. Local Git Hooks (`pre-commit`)

### Hooks
* **Standard hooks**: Trailing whitespace, EOF fixer, YAML/TOML parser, and large files filter.
* **`ruff`**: Runs the Ruff linter, performing auto-fixes where possible.
* **`ruff-format`**: Formats python files using Ruff's formatter.
* **`mypy`**: Checks static typings inside Python files.
* **`gitleaks`**: Scans files for hardcoded secrets, API tokens, and credentials.

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
* **Ruff Linting**: Validates imports, code safety, and standards.
* **Ruff Formatting**: Verifies files comply with PEP 8 layout styles.
* **Pytest Run**: Tests the application and records test results.
* **Anti-Skipped-Tests Enforcement**: Parses `pytest-report.xml` and fails the build if any test was skipped. This guarantees that all code tests are actually executed rather than bypassed.

### Setup
1. Copy the workflow file to your project:
   ```bash
   mkdir -p ../../.github/workflows/
   cp .github/workflows/ci.yml ../../.github/workflows/ci.yml
   ```
2. Make sure you have a `requirements.txt` or adapt the installation step if using toolchains like poetry/pipenv.
