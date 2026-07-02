# pre-commit

Specification: https://pre-commit.com/

## Requirements
* Python 3.8+ or Go (for `prek`)
* Git installed in your path

## Tools one can use:
* **[pre-commit](https://github.com/pre-commit/pre-commit)** (Official python-based package)
  * Install via pip: `pip install pre-commit` (or `pipx install pre-commit`)
  * Install via Homebrew: `brew install pre-commit`
* **[prek](https://github.com/j178/prek)** (Fast, recommended alternative runner in Go)
  * Install via Go: `go install github.com/j178/prek@latest`

## Supplement Commands (Usage & Testing)
* **Install hooks into your repo**:
  ```bash
  pre-commit install
  ```
* **Run against staged files (before commit)**:
  ```bash
  pre-commit run
  ```
* **Run manually on all files in the repository**:
  ```bash
  pre-commit run --all-files
  ```
* **Run a specific hook (e.g. gitleaks)**:
  ```bash
  pre-commit run gitleaks --all-files
  ```
* **Auto-update all hook versions in config**:
  ```bash
  pre-commit autoupdate
  ```
* **Uninstall hooks**:
  ```bash
  pre-commit uninstall
  ```

---

# GitHub Actions

Specification: https://docs.github.com/en/actions

## Requirements
* A Container Host: **Podman** (default) or **Docker**

## Tools to test it locally:
* **[act](https://github.com/nektos/act)** (Run GitHub Actions locally in containers)
  * Install via Homebrew: `brew install act`
  * Install via Curl: `curl --proto '=https' --tlsv1.2 -sSf https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo sh`

## Testing locally with Podman (Rootless Setup)
Since Podman is the default container host in this environment:
1. Ensure the Podman systemd user socket is running:
   ```bash
   systemctl --user enable --now podman.socket
   ```
2. Get the path to your user socket:
   ```bash
   podman info --format '{{.Host.RemoteSocket.Path}}'
   # Usually output is: /run/user/1000/podman/podman.sock
   ```
3. Set the `DOCKER_HOST` environment variable to point to the socket and run:
   ```bash
   export DOCKER_HOST=unix:///run/user/1000/podman/podman.sock
   act
   ```
   Or pass it as a command argument:
   ```bash
   act --container-daemon-socket unix:///run/user/1000/podman/podman.sock
   ```

## Supplement act Commands
* **List all available jobs**:
  ```bash
  act -l
  ```
* **Run a specific job**:
  ```bash
  act -j <job_id>
  ```
* **Simulate a pull request trigger**:
  ```bash
  act pull_request
  ```
* **Dry run (only show what would run)**:
  ```bash
  act -n
  ```

