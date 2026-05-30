---
preset: azure-ml
label: "Azure ML"
description: "End-to-end ML project targeting Azure Machine Learning: AzureML pipelines, MLflow tracking, managed endpoints for serving, Python + uv, GitHub Actions CI/CD."
---

# Preset: Azure ML

## Pre-filled answers

### Repo Structure
- **Topology:** Polyrepo

### Tech Stack
- **Language / Runtime:** Python
- **Build tooling:** uv
- **Database:** None (data in Azure Blob Storage / ADLS Gen2; feature store optional)
- **API style:** REST / HTTP (AzureML managed online endpoint)
- **Delivery target:** Library / SDK (ML pipelines + serving — no browser front-end)
- **Unit testing:** pytest
- **E2E / integration testing:** None (pipeline integration via AzureML SDK v2 in CI)
- **Linting & formatting:** Ruff
- **Cloud:** Yes — cloud-first design; Azure

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** No preference
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions
- **IaC:** Azure Bicep (AzureML workspace, compute clusters, storage)

## Typical project layout

```
data/                         # local sample data (full sets in ADLS)
src/
  <package>/
    components/               # AzureML pipeline components (YAML + Python)
      prep/
      train/
      evaluate/
      register/
    pipelines/                # AzureML pipeline definitions (SDK v2)
    serve/                    # scoring script for managed endpoint
    utils/
tests/
  unit/
  integration/                # small AzureML SDK smoke tests
environments/
  train.yml                   # conda env spec for training component
  serve.yml
infra/
  main.bicep                  # AzureML workspace + compute
  parameters/
    dev.json
    prod.json
.azure/
  pipelines/                  # Azure DevOps alt (keep for reference)
pyproject.toml
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| AzureML SDK v2 | Pythonic API for jobs, pipelines, endpoints; replaces v1 |
| MLflow (via AzureML) | Experiment tracking + model registry; no extra service needed |
| AzureML Managed Endpoints | Serverless model serving with autoscale; no k8s required |
| ADLS Gen2 | Hierarchical namespace for large datasets; native AzureML mount |
| uv | Fast, reproducible local Python env matching component envs |
| Azure Bicep | Native ARM IaC; cleaner than raw JSON; AzureML resource support |
| pytest | Unit-test feature transforms and scoring functions locally |
| GitHub Actions | Trigger `az ml job create` on PR; deploy endpoint on merge |
