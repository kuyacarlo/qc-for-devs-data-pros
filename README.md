# Quality Checks for Devs & Data Pros

Workshop/template materials for adding pre-commit and GitHub Actions quality gates to development and data repositories. This repository should stay usable as a template, not archived as a dead hackathon artifact.

## Layout

| Path | What |
|------|------|
| [`examples/base`](examples/base) | Language-agnostic hooks + Gitleaks CI |
| [`examples/python`](examples/python) | Python-oriented hooks with Ruff, Mypy, and Gitleaks |
| [`examples/js`](examples/js) | JS/TS-oriented hooks with Prettier, ESLint, and Gitleaks |
| [`examples/golang`](examples/golang) | Go-oriented hooks with golangci-lint, gofmt/goimports, and Gitleaks |
| [`presentation`](presentation) | Marp slides (`pnpm watch`) |

## Use an example

From the repository you want to protect, copy one example set into the target repo root. For example, from this template repository:

```bash
cp examples/base/.pre-commit-config.yml /path/to/target/.pre-commit-config.yml
mkdir -p /path/to/target/.github/workflows
cp examples/base/.github/workflows/ci.yml /path/to/target/.github/workflows/ci.yml
cd /path/to/target
pre-commit install
pre-commit run --all-files
```

Pick `python`, `js`, or `golang` instead of `base` when you want language-specific hooks.

## Validate this template repository

Safe local checks that do not call external scanners or deploy anything:

```bash
pre-commit validate-config examples/base/.pre-commit-config.yml
pre-commit validate-config examples/python/.pre-commit-config.yml
pre-commit validate-config examples/js/.pre-commit-config.yml
pre-commit validate-config examples/golang/.pre-commit-config.yml
python - <<'PY'
from pathlib import Path
required = [
    'examples/base/.github/workflows/ci.yml',
    'examples/python/.github/workflows/ci.yml',
    'examples/js/.github/workflows/ci.yml',
    'examples/golang/.github/workflows/ci.yml',
]
missing = [p for p in required if not Path(p).exists()]
if missing:
    raise SystemExit(f'missing workflow templates: {missing}')
print('workflow templates present')
PY
```

## Slides

```bash
cd presentation
pnpm install
pnpm watch
```

## License

[MIT](LICENSE)
