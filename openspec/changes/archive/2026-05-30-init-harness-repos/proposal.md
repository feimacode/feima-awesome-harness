## Why

Two new repositories need initialization to implement the feima harness ecosystem: `feima-awesome-harness` (official content repo) and `feima-harness-catalog` (central discovery service). The content repo needs multi-platform plugin manifests for Claude Code, Cursor, and Codex, plus the universal skills.sh distribution path. The catalog service needs the aggregation infrastructure to merge catalog entries into a discoverable index for `@flow` users.

This establishes the foundation for the flow orchestration ecosystem described in CATALOG_ECOSYSTEM.md, enabling users to discover and install flows via `@flow /browse` while also supporting platform-native installs from Claude Code, Copilot CLI, Cursor, and Codex.

## What Changes

### feima-awesome-harness (content repo)

- Create `.claude-plugin/marketplace.json` and `.claude-plugin/plugin.json` for Claude Code + Copilot CLI distribution
- Create `.cursor-plugin/plugin.json` for Cursor IDE distribution
- Create `.codex-plugin/plugin.json` (with `interface` section for UI metadata) for Codex distribution
- Create `skills.sh.json` for universal skills layer (`npx skills add`)
- Remove the empty `catalogs/` directory (content repos don't hold catalog metadata in Model A)
- Create placeholder/example content: one skill, one agent, one flow to validate schemas
- Create `assets/` directory for Codex icons/branding
- Create `commands/` and `hooks/` directories if needed by platform manifests

### feima-harness-catalog (catalog service)

- Create `catalogs/feima-awesome-harness/catalog.json` (metadata describing feima's content)
- Create `catalogs/community/` directory structure for future contributors
- Create `index.json` (initially empty/placeholder, auto-built by CI)
- Create `.github/workflows/build-index.yml` for catalog aggregation CI
- Create `scripts/build-index.js` for merging catalog.json files into index.json
- Create `README.md` explaining registration process

## Capabilities

### New Capabilities

- `multi-platform-plugin-manifests`: Platform-specific plugin.json files for Claude Code, Cursor, and Codex with proper schema differences (Claude has marketplace.json + plugin.json, Cursor/Codex have single plugin.json, Codex has interface section)
- `catalog-aggregation-service`: Central catalog service that aggregates per-repo catalog.json entries into a unified index.json for `@flow` discovery
- `skills-sh-distribution`: Universal skills layer manifest for `npx skills add` cross-platform installation

### Modified Capabilities

None - this is initial setup.

## Impact

- Two new repositories initialized with complete directory structures
- Multi-platform install paths available from day one:
  - `claude plugin marketplace add feima/awesome-harness`
  - `cursor plugin add feima/awesome-harness`
  - `codex plugin add feima/awesome-harness`
  - `npx skills add feima/awesome-harness`
- `@flow /browse` can fetch `index.json` from catalog service once CI is wired
- Content defined once in `skills/`, `agents/`, `flows/` directories, referenced by all platform manifests