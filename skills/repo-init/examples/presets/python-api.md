---
preset: python-api
label: "Python API"
description: "FastAPI service with PostgreSQL, uv, pytest, Ruff, and GitHub Actions. Opinionated modern Python API setup with no legacy tooling."
---

# Preset: Python API

## Pre-filled answers

### Repo Structure
- **Topology:** Polyrepo

### Tech Stack
- **Language / Runtime:** Python
- **Build tooling:** uv
- **Database:** Relational / RDBMS — PostgreSQL
- **API style:** REST / HTTP
- **Delivery target:** Library / SDK (API service — no browser front-end in this repo)
- **Unit testing:** pytest
- **E2E / integration testing:** None (add if needed)
- **Linting & formatting:** Ruff (lint + format)
- **Cloud:** No (override for cloud deploy)

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** No preference
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions
- **IaC:** Docker Compose only (local dev + Postgres)

## Typical project layout

```
src/
  <package>/
    main.py        # FastAPI app factory
    routes/
    models/
    schemas/
    services/
tests/
  unit/
  integration/
pyproject.toml     # uv-managed, all deps here
Dockerfile
docker-compose.yml # API + Postgres for local dev
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| FastAPI | Async-first, automatic OpenAPI docs, type-safe |
| uv | Fastest Python package/env manager; replaces pip + venv |
| pytest | De-facto standard; rich plugin ecosystem |
| Ruff | Single tool replaces Flake8 + isort + Black at 10x speed |
| PostgreSQL | Best RDBMS for Python; pairs well with SQLAlchemy/asyncpg |
| Docker Compose | Zero-setup local Postgres without a cloud account |
| GitHub Actions | Easy matrix testing across Python versions |
