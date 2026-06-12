# Feima Awesome Harness

A collection of reusable AI harnesses — skills, agents, flows, and prompts — for multi-platform AI-assisted development. Define once, use everywhere.

## What Is a Harness?

A **harness** is a reusable unit of AI instruction or configuration. It tells an AI assistant *how* to approach a task, *what role* to play, or *what workflow* to follow. Harnesses are platform-agnostic by design — the same skill or agent works across Claude Code, GitHub Copilot, Cursor, Codex, and any other AI tool that supports them.

This repository is the official collection. It ships as a single installable unit so you get everything in one step.

## Categories

| Category | Count | Description | Docs |
|----------|-------|-------------|------|
| **Skills** | 9 | Reusable knowledge and instruction files that guide AI behaviour for specific tasks | [docs/skills/](docs/skills/) |
| **Agents** | 1 | Role definitions with responsibilities, tone, and interaction patterns | [docs/agents.md](docs/agents.md) |
| **Flows** | 1 | Multi-role orchestration patterns that combine skills and agents into coordinated workflows | [docs/flows.md](docs/flows.md) |
| **Prompts** | 1 | Reusable templates for consistent, structured AI output | [docs/prompts.md](docs/prompts.md) |

### Skills at a Glance

| Group | Skills |
|-------|--------|
| **Testing** | [auto-test](docs/skills/auto-test.md) · [unit-test](docs/skills/unit-test.md) · [integration-test](docs/skills/integration-test.md) · [e2e-test](docs/skills/e2e-test.md) · [contract-test](docs/skills/contract-test.md) · [performance-test](docs/skills/performance-test.md) |
| **Project Setup** | [repo-init](docs/skills/repo-init.md) · [ai-config](docs/skills/ai-config.md) |
| **Incident Response** | [incident-triage](docs/skills/incident-triage.md) |

See [docs/skills/](docs/skills/) for detailed documentation on each skill.

## Installation

Install via your preferred platform:

| Platform | Install Command | What You Get |
|----------|-----------------|--------------|
| **Claude Code** | `claude plugin marketplace add feima/awesome-harness` | Skills + agents + hooks |
| **Copilot CLI** | `copilot plugin marketplace add feima/awesome-harness` | Skills + agents + hooks |
| **Cursor** | `cursor plugin add feima/awesome-harness` | Skills + agents + commands |
| **Codex** | `codex plugin add feima/awesome-harness` | Skills + interface |
| **Universal** | `npx skills add feima/awesome-harness` | Skills only (cross-platform) |

## Quick Start

### Use a Skill

Skills activate automatically when you mention their trigger phrases. For example:

- *"Write unit tests for this function"* → loads `unit-test` (and `auto-test` as prerequisite)
- *"Set up AI configs for this repo"* → loads `ai-config`
- *"We have a production outage"* → loads `incident-triage`

### Use an Agent

Agents define a persona for the AI to adopt. Reference them in your instructions:

- *"Act as the incident-commander and coordinate the response"*

### Use a Flow

Flows orchestrate multiple roles in sequence. They are executed by a flow runtime (such as the `@flow` VS Code extension):

- `war-room-triage` — runs a 3-stage incident investigation with a commander and four specialist roles

### Use a Prompt

Prompts are templates you fill in or ask the AI to fill in:

- *"Generate a status update using the status-update template"*

## Architecture

### Content-Only Repository

This repository contains **content only** (skills, agents, flows, prompts). Catalog metadata lives in the [feima-harness-catalog](https://github.com/feimacode/harness-catalog) service. This separation:

- Keeps content repos focused on content
- Makes catalog discovery a single source of truth
- Lowers friction for authors (no catalog metadata to maintain)

### Flat, Single-Unit Distribution

The whole repository is one installable unit. Install `feima/awesome-harness` and get all official content. Simpler than plugin-based structures where each harness is a separate package.

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
│   │   ├── examples/presets/  ← 11 stack presets
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
├── docs/                      ← Detailed documentation
│   ├── skills/                ← Per-skill docs
│   ├── agents.md
│   ├── flows.md
│   └── prompts.md
├── commands/                  ← Cursor commands (extensible)
├── hooks/                     ← Platform hooks
├── assets/                    ← Codex UI assets (icons, branding)
└── openspec/                  ← OpenSpec change management
```

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

3. Add a documentation page in `docs/skills/<skill-name>.md`

4. Submit PR — catalog metadata will be updated in feima-harness-catalog

### Adding Agents

1. Create `agents/<agent-name>.agent.md` — define the role, responsibilities, tone, and constraints

2. Add an entry in `docs/agents.md`

3. Submit PR

### Adding Flows

1. Create `flows/<flow-name>.flow.yaml` following the [flow schema](https://github.com/feimacode/copilot-ai-flow/blob/main/schemas/flow.schema.json)

2. Reference existing skills and agents by name

3. Add an entry in `docs/flows.md`

4. Submit PR — flows are validated against schema

### Adding Prompts

1. Create `prompts/<prompt-name>.prompt.md` — define the template structure and usage guidelines

2. Add an entry in `docs/prompts.md`

3. Submit PR

## Related Projects

| Project | Purpose |
|---------|---------|
| [feima-harness-catalog](https://github.com/feimacode/harness-catalog) | Central catalog aggregation service — indexes harnesses from this repo and community contributions |
| [copilot-ai-flow](https://github.com/feimacode/copilot-ai-flow) | `@flow` VS Code extension — a runtime that can execute flows defined in this repository |

## License

MIT — use freely, modify openly, attribute kindly.