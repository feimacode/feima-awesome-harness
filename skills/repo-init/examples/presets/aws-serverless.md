---
preset: aws-serverless
label: "AWS Serverless"
description: "Event-driven serverless application on AWS: Lambda + API Gateway + DynamoDB + SQS, provisioned with AWS CDK (TypeScript), tested with Vitest and AWS CDK Assertions."
---

# Preset: AWS Serverless

## Pre-filled answers

### Repo Structure
- **Topology:** MonoRepo
- **Monorepo tooling:** npm workspaces + Turborepo tasks

### Tech Stack
- **Language / Runtime:** Node.js / TypeScript
- **Build tooling:** npm (workspaces) + esbuild (Lambda bundling via CDK)
- **Database:** Key-Value / Cache — DynamoDB (primary); optionally PostgreSQL via RDS Data API
- **API style:** REST / HTTP (API Gateway v2 HTTP API)
- **Delivery target:** Library / SDK (serverless functions — no browser front-end in this repo)
- **Unit testing:** Vitest
- **E2E / integration testing:** None (integration covered by CDK integration tests + AWS SDK in CI)
- **Linting & formatting:** ESLint + Prettier
- **Cloud:** Yes — cloud-first design; AWS

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** bash
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions
- **IaC:** AWS CDK (TypeScript)

## Typical project layout

```
packages/
  functions/
    src/
      handlers/      # Lambda handler entry points
      services/      # business logic (no AWS SDK imports)
      repositories/  # DynamoDB access layer
    tests/
    package.json
  infra/             # AWS CDK app
    bin/app.ts
    lib/
      api-stack.ts
      data-stack.ts
    tests/           # CDK Assertions unit tests
    package.json
turbo.json
package.json         # workspaces root
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| AWS CDK (TypeScript) | Type-safe IaC; CDK constructs for Lambda/API Gateway/DynamoDB |
| API Gateway v2 HTTP API | Lower latency and cost than REST API; sufficient for most cases |
| DynamoDB | Serverless-native; no connection pool management |
| esbuild via CDK | Sub-second Lambda bundle build; tree-shaking for cold start size |
| Vitest | Fast unit tests for handler logic; ESM-native |
| CDK Assertions | Test infrastructure shapes without deploying |
| Turborepo | Caches function builds and CDK synth across packages |
| GitHub Actions | `cdk deploy` on merge to main; `cdk diff` on PR |
