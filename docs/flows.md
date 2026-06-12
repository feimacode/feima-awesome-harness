# Flows

Flows are multi-role orchestration patterns that combine skills and agents into coordinated workflows. They define the sequence, iteration, and interaction between roles to accomplish complex tasks.

## Available Flows

### war-room-triage — War Room Triage

**Source**: [`flows/war-room-triage.flow.yaml`](../flows/war-room-triage.flow.yaml)

#### Purpose

Orchestrates a multi-role incident triage across three stages. The Incident Commander coordinates while specialists investigate different layers of the system in parallel.

#### Orchestration Pattern

**Staged** — 3 stages with configurable iterations:

```
Stage 1: Initial Assessment (1 iteration)
  ├── commander — Gather context, assign investigation tasks
  └── recent-changes — Review recent commits, deployments, config changes

Stage 2: Deep Investigation (2 iterations)
  ├── commander — Synthesize findings, update incident picture
  ├── app-layer — Investigate code paths, error handling, business logic
  ├── infra-layer — Investigate servers, containers, networking, resources
  └── data-layer — Investigate databases, caches, storage

Stage 3: Resolution (1 iteration)
  └── commander — Final synthesis, confirm containment, propose post-incident actions
```

#### Roles

| Role | Type | Purpose |
|------|------|---------|
| commander | Agent (`incident-commander`) | Coordinates response, synthesizes findings |
| recent-changes | Skill (`incident-triage`) | Identifies potential triggers from recent changes |
| app-layer | Skill (`incident-triage`) | Investigates application code and error paths |
| infra-layer | Skill (`incident-triage`) | Investigates infrastructure and resource issues |
| data-layer | Skill (`incident-triage`) | Investigates database and storage issues |

#### Context Injection

The flow injects `git_history` as context, giving all roles access to recent commit history for change analysis.

#### When to Use

- Production incident with unclear root cause
- Multi-service outage requiring coordinated investigation
- Incidents where different layers (app, infra, data) may all be involved

#### Related Harnesses

- [incident-commander](agents.md#incident-commander) — Agent used as the commander role
- [incident-triage](skills/incident-triage.md) — Skill used by all specialist roles
- [status-update](prompts.md) — Prompt template for commander's stakeholder updates
