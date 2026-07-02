# Golang Quality Checks (Pre-Commit & GitHub Actions)

This folder contains pre-commit and CI setups tailored for Go codebases.

## 📁 Included Files
* **`.pre-commit-config.yml`**: Config for local hook validation.
* **`.github/workflows/ci.yml`**: Config for remote GitHub Actions pipeline.

---

## ⚡ 1. Local Git Hooks (`pre-commit`)

### Hooks
* **Standard checks**: Whitespace, EOF, YAML parsing, and large file checks.
* **`golangci-lint`**: Standard runner checking Go code against multiple linters.
* **`go fmt` (local)**: Formats your Go files.
* **`go-imports` (local)**: Automatically cleans up and organizes your import declarations.
* **`gitleaks`**: Scans for secrets before committing.

### Requirements
* Go toolchain installed locally (`go` command available in your PATH).
* `goimports` binary installed:
  ```bash
  go install golang.org/x/tools/cmd/goimports@latest
  ```

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
* **Linting**: Runs the official `golangci-lint-action` which executes a wide range of popular Go static code analysis linters.
* **Go Test**: Runs standard Go unit tests.
* **Anti-Skipped-Tests Enforcement**: Filters test logs for `--- SKIP:` prefixes and fails the build if any test was skipped. This guards against developers or automated tools bypassing test suites.

### Setup
1. Copy the workflow file to your project:
   ```bash
   mkdir -p ../../.github/workflows/
   cp .github/workflows/ci.yml ../../.github/workflows/ci.yml
   ```
2. Make sure you have a `go.mod` file in your repository root, or adjust the `working-directory` inside the workflow jobs if Go code sits in a subdirectory.
