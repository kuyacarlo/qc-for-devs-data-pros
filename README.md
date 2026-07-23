# Quality Checks for Devs & Data Pros

Workshop materials: pre-commit + GitHub Actions templates that catch file hygiene issues and secrets before they hit remote.

## Layout

| Path | What |
|------|------|
| [`examples/base`](examples/base) | Language-agnostic hooks + Gitleaks CI |
| [`examples/python`](examples/python) | Python-oriented checks |
| [`examples/js`](examples/js) | JS/TS-oriented checks |
| [`examples/golang`](examples/golang) | Go-oriented checks |
| [`presentation`](presentation) | Marp slides (`pnpm watch`) |

## Use an example

```bash
cp examples/base/.pre-commit-config.yml .pre-commit-config.yml
mkdir -p .github/workflows
cp examples/base/.github/workflows/ci.yml .github/workflows/ci.yml
pnpm dlx pre-commit install   # or: pip install pre-commit && pre-commit install
pre-commit run --all-files
```

Pick `python`, `js`, or `golang` instead of `base` when you want language-specific hooks.

## Slides

```bash
cd presentation
pnpm install
pnpm watch
```

## License

[MIT](LICENSE)
