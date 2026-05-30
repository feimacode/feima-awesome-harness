---
preset: web-api-monorepo-ts
label: "Web + API monorepo (TypeScript)"
description: "React 19 SPA + Node.js/Express API in a Turborepo monorepo. The most common full-stack TypeScript setup for teams shipping a browser app backed by a REST API."
---

# Preset: Web + API MonoRepo (TypeScript)

## Pre-filled answers

### Repo Structure
- **Topology:** MonoRepo
- **Monorepo tooling:** Turborepo (task orchestration) + npm workspaces

### Tech Stack
- **Language / Runtime:** Node.js / TypeScript
- **Build tooling:** npm (workspaces) + Turborepo tasks
- **Database:** Relational / RDBMS — PostgreSQL
- **API style:** REST / HTTP
- **Delivery target:** Web (browser) — React SPA + Node.js API back-end
  - CSS: Tailwind CSS v4
  - State / data fetching: TanStack Query (server-state only)
  - Routing: TanStack Router
  - UI component library: shadcn/ui
- **Unit testing:** Vitest
- **E2E testing:** Playwright
- **Linting & formatting:** ESLint + Prettier
- **Cloud:** No (override if deploying to AWS/GCP/Azure)

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** No preference
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions
- **IaC:** Docker Compose only (local dev)

## Typical package layout

```
apps/
  web/        # React 19 + Vite + Tailwind + TanStack
  api/        # Node.js + Express (or Hono/Fastify)
packages/
  ui/         # shadcn/ui components
  config/     # shared ESLint + TS configs
turbo.json
package.json  # workspaces root
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| npm workspaces | No extra tooling; Turborepo handles caching |
| Turborepo | Fast incremental builds, minimal config overhead |
| TanStack Query | Best-in-class async state; avoids Redux boilerplate |
| TanStack Router | Type-safe routing, first-class loader support |
| shadcn/ui | Copy-paste components, fully owned, Tailwind-compatible |
| Vitest | ESM-native, Vite-compatible, fast watch mode |
| Playwright | Reliable cross-browser E2E, strong TS support |
| ESLint + Prettier | Industry default; stable ecosystem |
| PostgreSQL | Proven RDBMS; works with Drizzle/Prisma/pg |
| GitHub Actions | Free for public repos; easy matrix/cache support |
