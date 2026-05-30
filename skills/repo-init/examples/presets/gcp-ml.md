---
preset: gcp-ml
label: "GCP ML"
description: "End-to-end ML project on Google Cloud: Vertex AI Pipelines, BigQuery feature store, MLflow or Vertex Experiments for tracking, Cloud Run for serving, Python + uv."
---

# Preset: GCP ML

## Pre-filled answers

### Repo Structure
- **Topology:** Polyrepo

### Tech Stack
- **Language / Runtime:** Python
- **Build tooling:** uv
- **Database:** None (data in GCS / BigQuery; Vertex Feature Store optional)
- **API style:** REST / HTTP (Cloud Run serving endpoint)
- **Delivery target:** Library / SDK (ML pipelines + serving — no browser front-end)
- **Unit testing:** pytest
- **E2E / integration testing:** None (pipeline integration via Vertex AI SDK in CI)
- **Linting & formatting:** Ruff
- **Cloud:** Yes — cloud-first design; Google Cloud

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** No preference
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions (Workload Identity Federation — no long-lived keys)
- **IaC:** Terraform / OpenTofu (Vertex AI, GCS, BigQuery, Cloud Run)

## Typical project layout

```
data/                          # local sample data (full sets in GCS/BQ)
src/
  <package>/
    components/                # Kubeflow/Vertex Pipeline components
      prep/
        component.py
        component.yaml
      train/
      evaluate/
      register/
    pipelines/
      training_pipeline.py     # Vertex AI Pipeline definition (KFP v2)
    serve/
      main.py                  # FastAPI app deployed to Cloud Run
      predictor.py
    utils/
tests/
  unit/
  integration/                 # Vertex AI SDK smoke tests
infra/
  main.tf
  modules/
    vertex/
    storage/
    bigquery/
  envs/
    dev.tfvars
    prod.tfvars
pyproject.toml
cloudbuild.yaml                # optional: Cloud Build trigger alternative
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| Vertex AI Pipelines (KFP v2) | Managed pipeline orchestration; component reuse; GCP-native |
| BigQuery | Serverless analytics; excellent for feature engineering at scale |
| Vertex AI Experiments / MLflow | Experiment tracking; model registry in Vertex Model Registry |
| Cloud Run | Serverless container serving; autoscale to zero; no k8s ops |
| GCS | Object storage for artefacts, datasets, and pipeline metadata |
| Workload Identity Federation | Keyless GCP auth from GitHub Actions; no service account JSON |
| Terraform | Mature GCP provider; manages Vertex + GCS + BQ + Cloud Run |
| uv | Fast, reproducible Python envs matching component container envs |
| pytest + Ruff | Standard Python quality tools; consistent with rest of ecosystem |
