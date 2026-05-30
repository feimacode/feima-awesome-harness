# Feima Awesome Harness

Official content repository for the feima harness ecosystem — flows, skills, agents, and prompts designed for AI-assisted development with multi-platform distribution.

## Mission

This repository exists to build a healthy content ecosystem that drives users toward **`@flow`** — the orchestration runtime that fills the gap in the AI agent equation.

```
Harness repo (skills, agents, flows)
    ↓  feeds
@flow runtime (orchestration, context injection, multi-role execution)
    ↓  creates
Outcomes that generic skills alone cannot produce
```

Skills are useful. Flows are transformative. A skill says "here are React best practices." A flow via `@flow` says "here are React best practices *applied to the specific component you're editing right now, with its current test failures and the last three commits that touched it.*"

## Installation

### Multi-Platform Support

Install via your preferred platform:

| Platform | Install Command | What You Get |
|----------|-----------------|--------------|
| **Claude Code** | `claude plugin marketplace add feima/awesome-harness` | Skills + agents + hooks |
| **Copilot CLI** | `copilot plugin marketplace add feima/awesome-harness` | Skills + agents + hooks |
| **Cursor** | `cursor plugin add feima/awesome-harness` | Skills + agents + commands |
| **Codex** | `codex plugin add feima/awesome-harness` | Skills + interface |
| **Universal** | `npx skills add feima/awesome-harness` | Skills only (cross-platform) |

### What's Included

- **9 Skills** — Reusable knowledge/instruction files
- **1 Flow** — Multi-role orchestration pattern
- **2 Agents/Prompts** — Role definitions for incident response
- **Commands & Hooks** — Platform-specific extensions

## Available Content

### Skills

#### Testing

| Skill | Purpose |
|-------|---------|
| `auto-test` | Universal testing principles for any automated test — structure, isolation, reliability, smells |
| `unit-test` | Unit tests with strict isolation, mocking philosophy, fixture design |
| `integration-test` | Tests for real integration boundaries (database, queue, HTTP) |
| `e2e-test` | End-to-end tests for user-facing flows through the full stack |
| `contract-test` | Contract tests for consumer-provider API agreements |
| `performance-test` | Performance tests measuring latency/throughput against SLO thresholds |

Use `auto-test` first for universal principles, then load the scope-specific skill for deeper guidance.

#### Project Setup

| Skill | Purpose |
|-------|---------|
| `repo-init` | Interactive wizard for initializing a new repository — tech stack, CI/CD, AI tools, presets for common stacks |
| `ai-config` | Manage AI assistant config files across tools (AGENTS.md, CLAUDE.md, copilot-instructions.md, .cursorrules, GEMINI.md) — detect drift, sync preferences |

#### Incident Response

| Skill | Purpose |
|-------|---------|
| `incident-triage` | Systematically triage production incidents — gather context, assess severity, propose containment |

### Flows

| Flow | Orchestration | Roles | Description |
|------|---------------|-------|-------------|
| `war-room-triage` | Staged (3 stages) | 6 | Multi-role incident triage: Commander coordinates, specialists investigate Recent Changes, App Layer, Infra Layer, Data Layer |

Flows are the orchestration layer that combines skills and agents into coordinated workflows. Run flows via `@flow` in VS Code with the Copilot AI Flow extension.

### Agents / Prompts

| Agent | Purpose |
|-------|---------|
| `incident-commander` | Coordinates incident response across multiple specialists — assigns tasks, synthesizes findings, maintains timeline |
| `status-update` | Template for incident status updates — clear, concise stakeholder communication |

## Architecture

### Model A — Catalog Metadata in Catalog Service

This repository contains **content only** (skills, agents, flows). Catalog metadata lives in the [feima-harness-catalog](https://github.com/feima/harness-catalog) service, not here. This separation:

- Keeps content repos focused on content
- Makes catalog discovery a single source of truth
- Lowers friction for authors (no catalog metadata to maintain)

### Flat Structure

The whole repository is one installable unit. Install `feima/awesome-harness` and get all official content. Simpler than plugin-based structures where each flow suite is a separate package.

### Multi-Platform Manifests

Platform-specific manifests point to the same content directories:

```
.claude-plugin/marketplace.json + plugin.json  → ./skills/, ./agents/, ./hooks/
.cursor-plugin/plugin.json                      → ./skills/, ./agents/, ./commands/
.codex-plugin/plugin.json                       → ./skills/ + interface metadata
skills.sh.json                                  → ./skills/ (universal distribution)
```

Content defined once, distributed to all platforms.

## Directory Structure

```
feima-awesome-harness/
├── .claude-plugin/
│   ├── marketplace.json       ← Claude Code + Copilot CLI marketplace
│   └── plugin.json            ← Content manifest
├── .cursor-plugin/
│   └── plugin.json            ← Cursor IDE manifest
├── .codex-plugin/
│   └── plugin.json            ← Codex manifest with interface section
├── skills.sh.json             ← Universal skills layer (npx skills add)
├── skills/
│   ├── auto-test/SKILL.md
│   ├── unit-test/SKILL.md
│   ├── integration-test/SKILL.md
│   ├── e2e-test/SKILL.md
│   ├── contract-test/SKILL.md
│   ├── performance-test/SKILL.md
│   ├── repo-init/SKILL.md
│   │   └── examples/presets/  ← 11 stack presets
│   │   └── references/        ← Platform tool mappings
│   ├── ai-config/SKILL.md
│   │   └── references/        ← Adapter + drift signal docs
│   └── incident-triage/SKILL.md
├── agents/
│   └── incident-commander.agent.md
├── flows/
│   └── war-room-triage.flow.yaml
├── prompts/
│   └── status-update.prompt.md
├── commands/                  ← Cursor commands (empty, extensible)
├── hooks/                     ← Platform hooks
├── assets/                    ← Codex UI assets (icons, branding)
└── openspec/                  ← OpenSpec change management
```

## Discovery

Find this harness and community contributions via `@flow /browse`:

```
@flow /browse incident
@flow /browse testing
@flow /browse --skills
```

The catalog service aggregates entries from this repo and community contributions into a unified index.

## Contributing

### Adding Skills

1. Create `skills/<skill-name>/SKILL.md` with YAML frontmatter:
   ```yaml
   ---
   name: your-skill-name
   description: "Brief description. Triggers on: 'trigger phrase', 'another trigger'."
   argument-hint: "Optional: mode or section hint"
   ---
   ```

2. Update `skills.sh.json` to include your skill in a grouping

3. Submit PR — catalog metadata will be updated in feima-harness-catalog

### Adding Flows

1. Create `flows/<flow-name>.flow.yaml` following the [flow schema](https://github.com/feima/copilot-ai-flow/blob/main/schemas/flow.schema.json)

2. Reference existing skills and agents by name

3. Submit PR — flows are validated against schema

### Adding Agents/Prompts

1. Create `agents/<agent-name>.agent.md` or `prompts/<prompt-name>.prompt.md`

2. Define the role, responsibilities, tone, and constraints

3. Submit PR

## Related Projects

| Project | Purpose |
|---------|---------|
| [feima-harness-catalog](https://github.com/feima/harness-catalog) | Central catalog aggregation service |
| [copilot-ai-flow](https://github.com/feima/copilot-ai-flow) | `@flow` VS Code extension — orchestration runtime |
| [CATALOG_ECOSYSTEM.md](https://github.com/feima/copilot-ai-flow/blob/main/docs/CATALOG_ECOSYSTEM.md) | Architecture documentation |

## License

MIT — use freely, modify openly, attribute kindly.