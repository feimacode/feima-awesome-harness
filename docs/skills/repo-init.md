# repo-init — Repository Initialization Wizard

**Category**: Project Setup &nbsp;|&nbsp; **Source**: [`skills/repo-init/SKILL.md`](../../skills/repo-init/SKILL.md)

## Purpose

Guides the user through a structured interview to establish all foundational decisions for a new repository. At the end, produces a reviewed summary and offers to generate config files, instructions, and CI/CD scaffolding.

## When to Use

- Initializing a brand-new repository
- Setting up project conventions and dev tooling
- Choosing a tech stack for a new project
- Configuring CI/CD pipelines from scratch
- Setting up AI coding assistant tooling for a repo

## Workflow

The wizard walks through five sections. Users can jump directly to any section or choose a preset to skip most questions.

### Fast-Track: Presets

11 stack presets are available to pre-fill all answers. The user only tweaks what differs:

| Preset | Stack |
|---|---|
| Web + API monorepo (TypeScript) | React 19 + Tailwind v4 + TanStack, Node.js API, Turborepo, Vitest, Playwright, PostgreSQL |
| Python API | FastAPI, uv, pytest, Ruff, GitHub Actions, PostgreSQL |
| TypeScript library / CLI | tsup, npm, Vitest, ESLint+Prettier, GitHub Actions npm publish |
| Java Microservices | Spring Boot 3 + Java 21, Maven multi-module, gRPC, Testcontainers, Helm |
| .NET Microservices | ASP.NET Core 9 minimal APIs, MassTransit, EF Core + Postgres, xUnit + Testcontainers |
| Python Data Science | Jupyter, pandas/polars, DVC, uv, Ruff, nbval CI smoke tests |
| Python ML Project | DVC pipelines, MLflow tracking, FastAPI serving, pytest, Docker |
| Go API Service | Chi router, sqlc + pgx, golang-migrate, Testcontainers-Go, golangci-lint |
| AWS Serverless | Lambda + API Gateway v2 + DynamoDB, AWS CDK (TypeScript), Turborepo |
| Azure ML | AzureML SDK v2 pipelines, MLflow, managed endpoints, Azure Bicep IaC |
| GCP ML | Vertex AI Pipelines (KFP v2), BigQuery, Cloud Run serving, Terraform |

### Interview Sections

1. **Repo Structure** — MonoRepo vs Polyrepo, monorepo tooling (Nx, Turborepo, Bazel, etc.)
2. **Tech Stack** — Language, build tool, database, API style, delivery target, testing frameworks, linting/formatting, cloud infrastructure
3. **AI Tools** — Coding assistant selection, development methodology, agent preferences
4. **CI/CD** — Platform (GitHub Actions, GitLab CI, etc.), Infrastructure as Code (Terraform, CDK, etc.)
5. **Review & Confirm** — Summary presentation and config generation

### Platform Adaptation

The wizard adapts its interaction style to the current AI platform (Copilot, Codex, Gemini CLI) using platform-specific tool mappings.

## References

- [Copilot CLI tool mapping](../../skills/repo-init/references/copilot-tools.md)
- [Codex tool mapping](../../skills/repo-init/references/codex-tools.md)
- [Gemini CLI tool mapping](../../skills/repo-init/references/gemini-tools.md)
- [Preset examples](../../skills/repo-init/examples/presets/)

## Related Skills

- [ai-config](ai-config.md) — Managing AI assistant config files after repo setup
