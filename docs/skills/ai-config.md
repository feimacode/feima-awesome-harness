# ai-config — AI Assistant Config Management

**Category**: Project Setup &nbsp;|&nbsp; **Source**: [`skills/ai-config/SKILL.md`](../../skills/ai-config/SKILL.md)

## Purpose

Detect, create, sync, and drift-check AI assistant configuration files across multiple tools. Works with whatever the user already has — never introduces a skill-owned state file.

## When to Use

- Initializing AI configs for a new repo (`AGENTS.md`, `CLAUDE.md`, `copilot-instructions.md`, etc.)
- Adding a config for a newly adopted AI tool
- Detecting drift between multiple co-existing AI config files
- Updating preferences across all active configs
- Detecting when tooling changes (linter, framework, test runner) are not yet reflected in AI configs

**Trigger phrases**: "set up AI configs", "add Cursor config", "my copilot instructions are out of date", "sync my AI configs", "check if my AI configs match the stack", "onboard Claude Code".

## Supported Platforms

| Tool | File(s) | Location |
|------|---------|----------|
| Shared / generic | `AGENTS.md` | repo root |
| Claude Code | `CLAUDE.md` | repo root (+ sub-dirs) |
| GitHub Copilot | `copilot-instructions.md` | `.github/` |
| Copilot file-scoped | `*.instructions.md` | `.github/instructions/` |
| Cursor | `*.mdc` rules | `.cursor/rules/` |
| Gemini CLI | `GEMINI.md` | repo root |
| Cline | `.clinerules` | repo root |
| Windsurf | `.windsurfrules` | repo root |
| Cursor legacy | `.cursorrules` | repo root |

## Modes

### `init` — First Config Setup

No configs exist. Guides the user through tool selection and preference gathering, then creates the most portable starting point (`AGENTS.md`).

### `add` — Onboard a New Tool

Configs exist for some tools. Adds a new tool's config while preserving existing preferences. Uses sentinel comments to mark generated sections without touching user-written content.

### `check` — Cross-Config Sync

Multiple configs exist. Runs a lightweight comparison to flag meaningful divergences (not formatting differences). Reports only actionable discrepancies.

### `edit` — Update Preferences

Updates a specific preference across all active configs. Ensures consistency without requiring manual edits to each file.

### `drift` — Tooling vs Config Consistency

Detects when the project's actual tooling (linter, framework, test runner) has changed but AI configs haven't been updated. Scans for signal files (e.g., `eslint.config.js`, `vitest.config.ts`) and compares against what configs describe.

## References

- [Adapter reference](../../skills/ai-config/references/adapters.md) — Per-tool file formats, locations, sentinel syntax
- [Drift signals reference](../../skills/ai-config/references/drift-signals.md) — Stack signal files and extraction rules

## Related Skills

- [repo-init](repo-init.md) — Repository setup wizard (often used before ai-config)
