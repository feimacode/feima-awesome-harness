---
preset: go-api
label: "Go API Service"
description: "Idiomatic Go REST API with Chi router, sqlc, PostgreSQL, golangci-lint, and GitHub Actions. Clean architecture with generated type-safe SQL — no ORM magic."
---

# Preset: Go API Service

## Pre-filled answers

### Repo Structure
- **Topology:** Polyrepo

### Tech Stack
- **Language / Runtime:** Go
- **Build tooling:** go build (built-in) + Makefile for convenience targets
- **Database:** Relational / RDBMS — PostgreSQL (via pgx + sqlc)
- **API style:** REST / HTTP
- **Delivery target:** Library / SDK (API service — no browser front-end)
- **Unit testing:** testing (built-in) + Testify
- **E2E / integration testing:** None (integration tests use Testcontainers-Go against real Postgres)
- **Linting & formatting:** golangci-lint + gofmt (built-in)
- **Cloud:** No (override for cloud deploy)

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** bash
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions
- **IaC:** Docker Compose only (local Postgres)

## Typical project layout

```
cmd/
  api/
    main.go          # wire up server, config, DI
internal/
  handler/           # HTTP handlers (Chi routes)
  service/           # business logic
  repository/        # sqlc-generated + wrappers
  middleware/
db/
  migrations/        # golang-migrate SQL files
  queries/           # .sql files consumed by sqlc
  sqlc.yaml
pkg/                 # exported, reusable packages
Dockerfile
docker-compose.yml   # API + Postgres
Makefile             # build, migrate, generate, test targets
go.mod
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| Chi | Lightweight, idiomatic, stdlib-compatible router |
| sqlc | Compile-time type-safe SQL — no runtime reflection |
| pgx | High-performance native Postgres driver |
| golang-migrate | Simple, file-based SQL migrations |
| Testify | Readable assertions without heavyweight framework |
| Testcontainers-Go | Real Postgres integration tests in CI |
| golangci-lint | Meta-linter; catches many classes of bugs statically |
| Makefile | Reproducible dev tasks without a separate task runner |
| GitHub Actions | go test + golangci-lint in parallel; fast feedback |
