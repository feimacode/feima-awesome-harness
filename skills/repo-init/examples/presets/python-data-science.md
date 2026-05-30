---
preset: python-data-science
label: "Python Data Science"
description: "Reproducible data analysis and visualisation project. Jupyter-centric with DVC for data versioning, pandas/polars, and GitHub Actions for notebook smoke tests."
---

# Preset: Python Data Science

## Pre-filled answers

### Repo Structure
- **Topology:** Polyrepo

### Tech Stack
- **Language / Runtime:** Python
- **Build tooling:** uv (environment + deps)
- **Database:** None (data stored as Parquet/CSV via DVC; override for SQL source)
- **API style:** None / internal only
- **Delivery target:** Library / SDK (notebooks + scripts — no web front-end)
- **Unit testing:** pytest
- **E2E / integration testing:** None
- **Linting & formatting:** Ruff
- **Cloud:** No (override to add cloud storage backend for DVC)

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** No preference
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions (runs nbval to smoke-test notebooks on push)
- **IaC:** None

## Typical project layout

```
data/
  raw/          # tracked by DVC, not git
  processed/
notebooks/
  01-eda.ipynb
  02-analysis.ipynb
src/
  <package>/
    data/       # loaders, transformers
    viz/        # reusable plot helpers
tests/
reports/        # generated figures, HTML exports
pyproject.toml  # uv-managed
dvc.yaml        # pipeline stages
params.yaml     # experiment parameters
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| uv | Fast, reproducible Python envs; replaces conda for most use-cases |
| pandas + polars | pandas for familiarity; polars for large-file performance |
| DVC | Data + model versioning alongside code without bloating git |
| Jupyter | Standard interactive analysis environment |
| Ruff | Single fast linter for notebooks (via nbqa) and scripts |
| pytest + nbval | Runs notebooks as tests to catch broken analyses in CI |
| Parquet | Columnar format; 10–100× faster than CSV for large datasets |
