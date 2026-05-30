---
name: repo-init
description: "Interactive repo setup wizard. Use when: initializing a new repository, setting up project conventions, configuring dev tooling, choosing tech stack, setting up CI/CD, configuring AI tools. Guides user through structured questions covering repo structure (monorepo/polyrepo), tech stack (language, database, API, frontend, testing), AI tools (agents, methodology), and CI/CD (pipelines, IaC). Produces a reviewed summary and generates config files."
argument-hint: "Optional: pass a specific section to jump to (repo-structure, tech-stack, ai-tools, ci-cd)"
---

# Repo Init Wizard

## Purpose

Guide the user through a structured interview to establish all foundational decisions for a new repository. At the end, produce a reviewed summary and offer to generate config files, instructions, and CI/CD scaffolding.

## Platform Adaptation

Before starting the interview, load the reference file for the current agent environment and follow its tool guidance:

- [Copilot CLI mapping](./references/copilot-tools.md)
- [Codex mapping](./references/codex-tools.md)
- [Gemini CLI mapping](./references/gemini-tools.md)

Prefer the platform's native structured question/input tool over plain chat whenever one exists. This skill should feel like a guided wizard, not a questionnaire dump.

---

## Defaults

Every question has a marked default `(default)`. If the user says **"skip"**, **"default"**, or gives a blank reply, accept the default and move on. Announce which default was taken so the user can backtrack if needed.

---

## Procedure

### Fasttrack — Presets

**Before Section 1**, ask the user if they want a head start:

> "Would you like a preset to skip most questions? I can pre-fill everything for a well-known stack and you only tweak what differs."

Available presets (load from `./examples/presets/`):

| Preset | File | What it sets up |
|--------|------|-----------------|
| **Web + API monorepo (TypeScript)** | `web-api-monorepo-ts.md` | React 19 + Tailwind v4 + TanStack Query/Router + shadcn/ui, Node.js API, npm workspaces + Turborepo, Vitest, Playwright, ESLint+Prettier, GitHub Actions, PostgreSQL |
| **Python API** | `python-api.md` | FastAPI, uv, pytest, Ruff, GitHub Actions, PostgreSQL |
| **TypeScript library / CLI** | `ts-library.md` | tsup, npm, Vitest, ESLint+Prettier, GitHub Actions npm publish |
| **Java Microservices** | `java-microservices.md` | Spring Boot 3 + Java 21, Maven multi-module, gRPC inter-service, Testcontainers, Helm, GitHub Actions |
| **.NET Microservices** | `dotnet-microservices.md` | ASP.NET Core 9 minimal APIs, MassTransit, EF Core + Postgres, xUnit + Testcontainers, Azure Bicep |
| **Python Data Science** | `python-data-science.md` | Jupyter, pandas/polars, DVC data versioning, uv, Ruff, nbval CI smoke tests |
| **Python ML Project** | `python-ml.md` | DVC pipelines, MLflow tracking, FastAPI serving, uv, Ruff, pytest, Docker |
| **Go API Service** | `go-api.md` | Chi router, sqlc + pgx, golang-migrate, Testcontainers-Go, golangci-lint, GitHub Actions |
| **AWS Serverless** | `aws-serverless.md` | Lambda + API Gateway v2 + DynamoDB, AWS CDK (TypeScript), Turborepo, Vitest, GitHub Actions |
| **Azure ML** | `azure-ml.md` | AzureML SDK v2 pipelines, MLflow, managed endpoints, Azure Bicep IaC, GitHub Actions |
| **GCP ML** | `gcp-ml.md` | Vertex AI Pipelines (KFP v2), BigQuery, Cloud Run serving, Terraform, Workload Identity Federation |
| **Start from scratch** | *(no preset)* | Walk through all sections manually |

If a preset is chosen: read the preset file, populate all answers from it, jump directly to **Section 5 — Review & Confirm**, and show the pre-filled summary. The user only answers questions they want to change.

If **"Start from scratch"** is chosen: continue with the full interview below.

If the user passes a section name as an argument, jump directly to that section.

> **At the very start of the interview**, tell the user: *"Feel free to ask me to explain any term at any time — just say something like 'what is X?' and I'll clarify before we continue. You can also say 'skip' or 'default' on any question to accept the default."*

---

### Section 1 — Repo Structure

1. **Repo topology** — MonoRepo or Polyrepo? *(not sure what these mean? just ask)*
   - Options: `MonoRepo`, `Polyrepo` *(default: Polyrepo)*

2. *(Only if MonoRepo)* **Monorepo tooling** — which tool manages the workspace?
   - Options: `Nx`, `Turborepo` *(default)*, `Bazel`, `Rush`, `Lerna`, `pnpm workspaces (no orchestrator)`, `Other`

Record answers, then proceed to Section 2.

---

### Section 2 — Tech Stack

Work through each sub-topic below in order. Options for later sub-topics adapt based on earlier answers. User can say "skip" on any sub-topic to accept the default.

#### 2a. Overall Language / Runtime

Options: `Java / JVM (Kotlin, Scala, etc.)`, `Python`, `Node.js / TypeScript` *(default)*, `.NET (C#, F#)`, `Go`, `Rust`, `Other`

#### 2b. Build Tooling

Present options **driven by 2a**:

| 2a answer | Options (default first) |
|-----------|--------------------------|
| Java / JVM | **Maven** *(default)*, Gradle, Bazel, Other |
| Python | **uv** *(default)*, Poetry, Hatch, pip + setuptools, Bazel, Other |
| Node.js / TypeScript | **npm** *(default)*, yarn, pnpm, Bun, Turborepo tasks, Other |
| .NET | **MSBuild / dotnet CLI** *(default)*, Cake, NUKE, Other |
| Go | **go build (built-in)** *(default)*, Mage, Task, Other |
| Rust | **Cargo (built-in)** *(default)*, Other |
| Other | Ask user to describe |

#### 2c. Database

Options (multi-select, default: `None`): `Relational / RDBMS (PostgreSQL, MySQL, SQLite…)`, `Document DB (MongoDB, Firestore…)`, `Key-Value / Cache (Redis, DynamoDB…)`, `Graph DB (Neo4j, Amazon Neptune…)`, `Time-Series (InfluxDB, TimescaleDB…)`, `Vector DB (pgvector, Pinecone, Qdrant…)`, **`None`** *(default)*

For each selected DB category, ask which specific engine (or "undecided").

#### 2d. API Style

Options (multi-select, default: `REST / HTTP`): **`REST / HTTP`** *(default)*, `GraphQL`, `gRPC / Protobuf`, `tRPC`, `WebSockets`, `None / internal only`

#### 2e. Delivery Target

Options: **`Web (browser)`** *(default)*, `Desktop`, `Mobile`, `CLI / daemon`, `Library / SDK`, `Multiple`

- **If Web:** ask about (each skippable):
  - CSS approach *(default: Tailwind CSS)*: **`Tailwind CSS`**, `Bootstrap / Bulma`, `CSS Modules`, `Styled Components / Emotion`, `Vanilla CSS`, `Other`
  - State management *(default: TanStack Query — server-state only)*: **`TanStack Query (server-state only)`**, `Redux / RTK`, `Zustand`, `MobX`, `Jotai / Recoil`, `None`, `Other`
  - UI component library *(default: shadcn/ui)*: **`shadcn/ui`**, `Radix UI`, `MUI`, `Ant Design`, `Chakra UI`, `None`, `Other`
  - Routing *(default: TanStack Router)*: **`TanStack Router`**, `React Router`, `Next.js App Router`, `None`, `Other`

- **If Desktop** *(default: Tauri)*:
  - Shell: `Native (Swift/AppKit, WinUI, GTK)`, `Webview — Electron`, **`Webview — Tauri`** *(default)*, `Webview — Neutralinojs`, `Other`

- **If Mobile** *(default: React Native)*:
  - Framework: **`React Native`** *(default)*, `Flutter`, `Native (Swift / Kotlin)`, `Capacitor / Ionic`, `Other`

#### 2f. Testing

- **Unit test framework** (driven by 2a, default noted per runtime):

| Runtime | Options (default first) |
|---------|--------------------------|
| Java / JVM | **JUnit 5** *(default)*, TestNG, Spock, Other |
| Python | **pytest** *(default)*, unittest, Other |
| Node.js / TypeScript | **Vitest** *(default)*, Jest, Node test runner, Other |
| .NET | **xUnit** *(default)*, NUnit, MSTest, Other |
| Go | **testing (built-in)** *(default)*, Testify, Other |
| Rust | **built-in test** *(default)*, Other |

- **E2E / integration framework** (multi-select, default: `None`): `Playwright`, `Cypress`, `Selenium / WebDriver`, `Puppeteer`, `Detox (mobile)`, `k6 (load)`, **`None`** *(default)*, `Other`

#### 2g. Linting & Formatting

Options driven by 2a (default noted per runtime):

| Runtime | Options (default first) |
|---------|--------------------------|
| Java / JVM | **Checkstyle + google-java-format** *(default)*, PMD, SpotBugs, Other |
| Python | **Ruff** *(default)*, Flake8 + isort + Black, Pylint, Other |
| Node.js / TypeScript | **ESLint + Prettier** *(default)*, Biome, StandardJS, Other |
| .NET | **Roslyn analyzers** *(default)*, StyleCop, Other |
| Go | **golangci-lint + gofmt** *(default)*, Other |
| Rust | **clippy + rustfmt (built-in)** *(default)*, Other |

#### 2h. Cloud & Infrastructure

- Cloud-native? *(default: No)*: `Yes — cloud-first design`, `Partially — some managed services`, **`No — on-prem / self-hosted`** *(default)*
- *(If Yes or Partially)* Cloud providers (multi-select, default: `AWS`): **`AWS`** *(default)*, `Google Cloud`, `Azure`, `Cloudflare`, `Other`

---

### Section 3 — AI Tools

#### 3a. AI Coding Assistant

Options (multi-select, default: `GitHub Copilot`): **`GitHub Copilot (VS Code agent mode)`** *(default)*, `Claude Code (Anthropic CLI)`, `Cursor`, `Google Gemini CLI / AI Studio`, `Codeium / Windsurf`, `None`, `Other`

#### 3b. AI Development Methodology

- Does the team follow a structured AI-driven spec methodology? *(default: No — ad-hoc)*
  - Options: `Yes — Spec-Driven Development (SDD)`, `Yes — custom internal methodology`, **`No — ad-hoc`** *(default)*, `Unsure`

- *(If SDD)* Spec format *(default: Open Spec)*: `Spec Kit (structured YAML/MD specs)`, **`Open Spec (Markdown-based)`** *(default)*, `Custom format`, `Other`

#### 3c. AI Agent Preferences

These will be recorded in the repo's agent instructions file (e.g. `copilot-instructions.md`, `AGENTS.md`, `.cursorrules`, or equivalent for the chosen assistant).

- **Shell preference** *(default: No preference)*: `bash`, `zsh`, `fish`, `PowerShell`, **`No preference`** *(default)*
- **Tool use style** *(default: No preference)*: `Prefer shell commands over MCP tools when equivalent`, `Prefer MCP tools`, **`No preference`** *(default)*
- **Confirmation behavior** *(default: Ask before destructive actions)*: **`Ask before destructive actions`** *(default)*, `Auto-approve safe ops, ask for destructive`, `Minimal confirmations`
- **Additional free-text preferences** (optional, skip to leave blank): e.g. "always use conventional commits", "never modify test files without asking"

---

### Section 4 — CI/CD

#### 4a. CI/CD Platform

Options: **`GitHub Actions`** *(default)*, `GitLab CI/CD`, `CircleCI`, `Jenkins`, `Azure DevOps`, `Bitbucket Pipelines`, `Buildkite`, `TeamCity`, `None yet`, `Other`

#### 4b. Infrastructure as Code (IaC)

Options (multi-select, default: `None`): `Terraform / OpenTofu`, `AWS CDK`, `Pulumi`, `AWS CloudFormation`, `Azure Bicep / ARM`, `Ansible`, `Helm (Kubernetes)`, `Docker Compose only`, **`None`** *(default)*, `Other`

---

### Section 5 — Review & Confirm

Present the complete summary in a structured format:

```
## Repo Init Summary

### Repo Structure
- Topology: <MonoRepo / Polyrepo>
- Monorepo tool: <if applicable>

### Tech Stack
- Language / Runtime: ...
- Build tool: ...
- Database(s): ...
- API style: ...
- Delivery target: ...
  - [Web] CSS: ... | State: ... | UI lib: ...
  - [Desktop] Shell: ...
- Unit testing: ...
- E2E testing: ...
- Linting: ...
- Cloud: ... | Providers: ...

### AI Tools
- Assistant(s): ...
- Methodology: ...
- Shell preference: ...
- Tool use style: ...
- Extra preferences: ...

### CI/CD
- Platform: ...
- IaC: ...
```

Ask the user to confirm or correct any item before proceeding.

---

### Section 6 — Next Steps

After confirmation, ask the user what they want to do next (multi-select):

- **Init the project right now** — scaffold the repo layout, config files, and tooling based on the choices above (monorepo config, linter/formatter config, build tool config, etc.)
- **Set up AI assistant configs** — generate `AGENTS.md` and tool-specific config files (`CLAUDE.md`, `copilot-instructions.md`, Cursor rules, `GEMINI.md`, etc.) using the `ai-config` skill. The assistant choices, shell preference, methodology, and extra conventions from Section 3 will be passed automatically — no re-answering needed.
- *(Only if SDD was selected in 3b)* **Install SDD Tools** — set up the spec-driven development toolchain and folder structure for the chosen spec format
- **Something else** — ask the user to describe what they need; proceed accordingly

Execute each selected action one by one. For any action that writes files, describe what will be created and confirm before writing.

#### Handoff to `ai-config` skill

When **"Set up AI assistant configs"** is selected, invoke the `ai-config` skill and pass the following context from Section 3 so it skips the questions already answered:

```
tools:         <list from 3a, e.g. [copilot, claude]>
shell:         <from 3c>
commit_style:  <from 3c extra preferences, if mentioned>
confirmation:  <from 3c>
extra:         <from 3c free-text preferences>
methodology:   <from 3b, e.g. spec-driven>
stack:
  runtime:     <from 2a>
  linter:      <from 2g>
  test:        <from 2f unit framework>
```

Announce to the user which values are being pre-filled before the `ai-config` skill starts, so they can correct anything without re-running the full wizard.

---

## Guidelines

- **One section at a time.** Present one section's questions, then wait for the user's answers before moving on.
- **Fasttrack first.** Always offer presets before starting Section 1. Loading a preset jumps directly to Section 5.
- **Skip = default.** If the user says "skip", "default", or leaves a reply blank, use the marked default and announce it: *"Using default: X"*.
- **Load the current platform reference first.** Use the matching file in `./references/` before the interview starts so you follow that agent's preferred question UX.
- **Prefer native question UI.** Use the platform's structured question/input tool when available. Fall back to plain chat only if the platform has no dedicated input mechanism.
- **One question per turn.** Even within a section, keep each prompt focused on a single decision.
- **Explain on demand.** If the user asks "what is X?" or "explain X" at any point, explain the term concisely, then re-present the current question so they can answer without losing their place.
- **Keep the main skill body platform-neutral.** Do not hardcode environment-specific tool names here; put those details in the reference files.
- **Adapt options to runtime.** Build tools, test frameworks, and linters shown must match the selected language.
- **Mark branches clearly.** Only ask conditional sub-questions when the parent condition is met.
- **Always include an "Other" option** with a free-text field so users aren't blocked by a missing option.
- **Multi-select where applicable.** Database, cloud providers, IaC, and AI assistants can have more than one answer. Use native multi-select UI when supported; otherwise collect a comma-separated or repeated-answer list and confirm the parsed values if needed.
- **Preserve all answers** across sections — the full state is needed for the summary and file generation.
- **Be concise in questions.** Show a short description per option, not paragraphs.
- **SDD state is sticky.** If the user selected SDD in Section 3b, always surface the "Install SDD Tools" option in Next Steps.
