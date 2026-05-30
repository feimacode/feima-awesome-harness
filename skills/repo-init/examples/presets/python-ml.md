---
preset: python-ml
label: "Python ML Project"
description: "End-to-end ML project from data prep to model serving. MLflow for experiment tracking, DVC for pipeline versioning, FastAPI for model serving, GitHub Actions CI."
---

# Preset: Python ML Project

## Pre-filled answers

### Repo Structure
- **Topology:** Polyrepo

### Tech Stack
- **Language / Runtime:** Python
- **Build tooling:** uv
- **Database:** None (artefacts via DVC; feature store optional)
- **API style:** REST / HTTP (model serving endpoint)
- **Delivery target:** Library / SDK (ML service + training pipeline — no browser front-end)
- **Unit testing:** pytest
- **E2E / integration testing:** None (model integration tested via DVC repro)
- **Linting & formatting:** Ruff
- **Cloud:** Partially — some managed services (S3/GCS for artefact storage)

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** No preference
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions (run DVC repro on PR, push model to registry on merge)
- **IaC:** Docker Compose only (local MLflow + Postgres metadata store)

## Typical project layout

```
data/
  raw/              # DVC-tracked
  processed/
models/             # DVC-tracked artefacts
notebooks/
  exploration/
src/
  <package>/
    data/           # ingestion, feature engineering
    train/          # training scripts, hyperparameter configs
    evaluate/       # metrics, comparison utilities
    serve/          # FastAPI model serving app
    pipelines/      # DVC pipeline stage definitions
tests/
  unit/
  integration/      # test inference endpoint
params.yaml         # hyperparameters referenced by DVC
dvc.yaml            # pipeline DAG
mlflow/             # local tracking server config
pyproject.toml
Dockerfile          # serving container
docker-compose.yml  # MLflow + Postgres for local tracking
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| uv | Fast, reproducible environments; avoids conda complexity |
| DVC | Reproducible ML pipelines; data + model versioning in git |
| MLflow | Experiment tracking, model registry, no vendor lock-in |
| FastAPI | Low-latency serving with automatic OpenAPI schema |
| scikit-learn / PyTorch | scikit for classical ML; PyTorch for deep learning |
| Ruff | Fast linting across training scripts and notebooks |
| pytest | Unit tests for feature transforms and data loaders |
| Docker | Reproducible training and serving environments |
| GitHub Actions | Run DVC repro in CI to catch regressions before merge |
