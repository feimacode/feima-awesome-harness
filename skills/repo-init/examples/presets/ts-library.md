---
preset: ts-library
label: "TypeScript library / CLI"
description: "Publishable npm package or standalone CLI tool. Minimal, modern TypeScript with tsup bundling, Vitest, and GitHub Actions release workflow."
---

# Preset: TypeScript Library / CLI

## Pre-filled answers

### Repo Structure
- **Topology:** Polyrepo

### Tech Stack
- **Language / Runtime:** Node.js / TypeScript
- **Build tooling:** npm + tsup (bundler)
- **Database:** None
- **API style:** None / internal only
- **Delivery target:** Library / SDK (or CLI / daemon)
- **Unit testing:** Vitest
- **E2E / integration testing:** None
- **Linting & formatting:** ESLint + Prettier
- **Cloud:** No

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** No preference
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions (test + publish to npm on tag)
- **IaC:** None

## Typical project layout

```
src/
  index.ts       # main entry point
  cli.ts         # (if CLI) commander/yargs entry
tests/
  index.test.ts
tsconfig.json
tsup.config.ts   # builds CJS + ESM outputs
package.json
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| tsup | Zero-config dual CJS/ESM output; esbuild-powered |
| Vitest | ESM-native, no Babel transform needed |
| ESLint + Prettier | Stable, widely understood |
| GitHub Actions | Built-in `NPM_TOKEN` secret → easy publish on tag push |
