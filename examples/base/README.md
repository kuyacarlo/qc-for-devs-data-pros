# Base Quality Checks (Pre-Commit & GitHub Actions)

This folder contains baseline configuration files suitable for any type of repository. It provides standard file sanity validations and deep-scan checks for secrets.

## 📁 Included Files
* **`.pre-commit-config.yml`**: Config for local hook validation.
* **`.github/workflows/ci.yml`**: Config for remote GitHub Actions pipeline.

---

## ⚡ 1. Local Git Hooks (`pre-commit`)

Runs locally on your machine before a commit is finalized.

### Hooks
* **`trailing-whitespace`**: Removes extra whitespaces at the end of lines.
* **`end-of-file-fixer`**: Ensures files end with a clean newline.
* **`check-yaml` / `check-toml` / `check-json`**: Validates configuration file syntax.
* **`check-added-large-files`**: Prevents accidental commits of files larger than 500KB.
* **`gitleaks`**: Runs a fast, local check on staged files to prevent credentials/secrets leak.

### Local Setup
1. Install pre-commit:
   ```bash
   pip install pre-commit
   ```
2. Copy configuration to repository root:
   ```bash
   cp .pre-commit-config.yml ../../.pre-commit-config.yml
   ```
3. Install git hooks:
   ```bash
   pre-commit install
   ```
4. Test by running:
   ```bash
   pre-commit run --all-files
   ```

---

## ☁️ 2. Remote Pipeline (GitHub Actions)

Runs automatically in the cloud on every push or pull request.

### Jobs
* **Lint & Scan Secrets**: Clones code with full history (`fetch-depth: 0`) and runs **Gitleaks** to scan all previous commits and check for password/API key exposures.

### Setup
1. Copy the workflow file to your project:
   ```bash
   mkdir -p ../../.github/workflows/
   cp .github/workflows/ci.yml ../../.github/workflows/ci.yml
   ```
2. Commit and push the file to GitHub.
