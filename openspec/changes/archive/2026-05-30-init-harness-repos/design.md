## Context

Two repositories need initialization to support the feima harness ecosystem:

1. **feima-awesome-harness** - The official content repository containing flows, skills, agents, and prompts. Must support multi-platform distribution via platform-native plugin manifests plus the universal skills.sh layer.

2. **feima-harness-catalog** - The central catalog aggregation service that merges per-repo catalog.json entries into a unified index.json for `@flow /browse` discovery.

The architecture follows **Model A** from CATALOG_ECOSYSTEM.md: each content repo owns its plugin manifests, but catalog metadata lives in the catalog service (not in each repo). The catalog service has `catalogs/<repo-name>/catalog.json` files that describe content from that repo.

Reference implementation exists in the superpowers repo (obra/superpowers) which has working `.claude-plugin/`, `.cursor-plugin/`, and `.codex-plugin/` directories.

## Goals / Non-Goals

**Goals:**
- Initialize both repos with complete directory structures
- Create multi-platform plugin manifests with correct schemas for each platform
- Set up catalog aggregation infrastructure (CI + scripts)
- Enable all four distribution paths from day one
- Create minimal example content to validate schemas work

**Non-Goals:**
- Creating production-quality flows/skills/agents (placeholder content only)
- Wiring up actual CI secrets/authentication (infrastructure setup)
- Implementing `@flow` extension changes (separate change)
- Supporting Model B architecture (repo-owned catalog.json at root)

## Decisions

### D1: Model A architecture (catalog metadata in catalog service)

**Rationale:** Model A separates content from metadata cleanly. Each harness repo focuses on its content (skills, agents, flows) without needing to maintain catalog metadata. The catalog service is the single source of truth for discovery.

**Alternatives considered:**
- Model B (each repo has catalog.json at root): Would require repos to maintain their own metadata and the catalog service to aggregate via `sources.json`. More distributed but higher friction for authors.
- Hybrid: Would create confusion about where metadata lives.

**Decision:** Model A for clarity and lower author friction.

### D2: Flat structure for feima-awesome-harness

**Rationale:** The whole repo is one installable unit. Users install `feima/awesome-harness` and get all official content. Simpler than plugin-based structure where each flow suite is a separate installable package.

**Alternatives considered:**
- Plugin-based (each flow in `plugins/<name>/` with its own plugin.json): More granular but adds complexity. Can be added later if needed.

**Decision:** Flat structure for simplicity.

### D3: Directory naming `catalogs/feima-awesome-harness/` instead of `_official/`

**Rationale:** The directory name IS the namespace. Self-documenting - no mental mapping from `_official` to "feima's content". Same pattern applies to all repos: `catalogs/<repo-name>/`.

**Decision:** Use repo name as directory name.

### D4: Platform manifest schema differences

**Claude Code:** Requires both `marketplace.json` (marketplace index) and `plugin.json` (content manifest). The marketplace concept allows multiple plugins per repo.

**Cursor:** Single `plugin.json` with directory references (`skills`, `agents`, `commands`) and file reference for hooks (`hooks/hooks-cursor.json`).

**Codex:** Single `plugin.json` with `interface` section for UI metadata (displayName, category, capabilities, brandColor, composerIcon, etc.).

**Decision:** Follow each platform's actual schema, derived from superpowers reference implementation.

### D5: Shared content directories across all platforms

**Rationale:** All three plugin.json files point to the same `./skills/`, `./agents/`, `./flows/` directories. Content defined once, referenced by all platforms.

**Decision:** Single content tree, multiple manifest files pointing to it.

### D6: Catalog discovery from plugin manifests (fallback mode)

**Rationale:** If a repo registers in the catalog but doesn't have an explicit `catalog.json` entry, the `build-index.js` script can derive metadata from their plugin manifests (marketplace.json, plugin.json, skills.sh.json).

**Decision:** Support both explicit catalog.json and auto-derived from manifests.

## Risks / Trade-offs

### R1: Platform schema changes → Catalog service needs updates

**Risk:** Claude Code, Cursor, or Codex may change their plugin.json schema format. The catalog service's manifest-to-catalog mapping would need updates.

**Mitigation:** Monitor superpowers repo (high-profile reference implementation) for schema changes. Update build-index.js when changes detected.

### R2: Codex interface metadata required for UI

**Risk:** Codex plugin.json requires `interface` section with brandColor, icons, etc. Missing this may cause poor UI presentation or rejection.

**Mitigation:** Include full `interface` section in `.codex-plugin/plugin.json` with placeholder assets. Create `assets/` directory for icons.

### R3: Empty catalog initially

**Risk:** `index.json` will be minimal/empty until CI is wired and runs build-index.js.

**Mitigation:** Create placeholder index.json with feima-awesome-harness entry. CI will replace it on first run.

### R4: Cross-platform hooks/commands directories

**Risk:** Cursor plugin.json references `commands/` and `hooks/`. Claude plugin.json may also reference hooks. These directories need to exist.

**Mitigation:** Create `commands/` and `hooks/` directories even if initially empty.

## Migration Plan

No migration needed - this is initial setup.

## Open Questions

1. Should `skills.sh.json` include all skills or subset? (skills.sh only handles skills, not agents/flows)
2. Should catalog.json entries reference source as `github:feima/awesome-harness` or individual Gists?
3. What placeholder content should be created to validate schemas? (one skill, one agent, one flow minimum)