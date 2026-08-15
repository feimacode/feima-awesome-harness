---
name: codebase-deep-dive
description: "Extract the idea, architecture, and implementation details of an existing project — a public GitHub repo or an internal/private repo — into a structured brief: what it does and for whom, its architecture and tech stack, how it's actually built (entry points, key modules, notable patterns, dependencies and why), and health/risk signals (maintenance activity, license, tech debt). For founders sizing up a fork candidate or learning from prior art, and architects onboarding to or borrowing patterns from an internal codebase. Works standalone (point it at any repo) or chained after solution-feasibility-study/solution-candidate-design to ground a specific fork candidate in real detail. Triggers on: 'explain this repo', 'how does this codebase work', 'deep dive into this project', 'understand the architecture of X', 'what can we learn from this GitHub repo', 'onboard me to this codebase', 'extract the architecture from this project'."
argument-hint: "Optional: paste a GitHub URL or local repo path directly to skip the intro question"
---

# Codebase Deep Dive

## Purpose

Take an existing project — public on GitHub, or an internal/private repo the user has access to — and extract a structured, grounded understanding of it: the product idea and who it's for, the architecture and tech stack, the concrete implementation details worth knowing (entry points, key modules, notable patterns, dependencies and why they're there), and health/risk signals (maintenance activity, license, tech debt, ownership concentration).

This is a research/extraction skill, not a code audit or a security review — for a full security pass, use `security-review` instead. It reads and reports; it never modifies the target repo, and never forks or clones it without asking first.

It's most useful in two places:
- **Standalone** — a founder or architect points it at any repo (GitHub URL or local path) they want to understand before using it as inspiration, a dependency, or a learning reference.
- **Chained** — after [`solution-feasibility-study`](../solution-feasibility-study/SKILL.md) surfaces fork candidates, or as part of [`solution-candidate-design`](../solution-candidate-design/SKILL.md)'s "Fork & Extend" candidate, this skill goes deep on *one* specific repo instead of the lighter README-level pass those skills do.

## Platform Adaptation

Before starting the interview, load the reference file for the current agent environment and follow its tool guidance (shared with `repo-init`, `solution-feasibility-study`, `audience-behavior-study`, and `solution-candidate-design`):

- [Copilot CLI mapping](../repo-init/references/copilot-tools.md)
- [Codex mapping](../repo-init/references/codex-tools.md)
- [Gemini CLI mapping](../repo-init/references/gemini-tools.md)

Prefer the platform's native structured question/input tool over plain chat whenever one exists.

---

## Procedure

### Step 0 — Intake

Check for context to reuse before asking anything:

- A `feasibility-study-*.md` or `solution-candidates-*.md` file in the working directory naming a specific fork candidate the user wants to go deeper on.
- A GitHub URL or local path pasted directly in the user's message or the skill argument.

Confirm in one line what was reused. Otherwise ask directly:

> "Which repo? A GitHub URL, or a local path if it's already cloned/internal. If it's a private internal repo I can't reach directly, paste the README and any architecture docs you have and I'll work from those."

### Step 1 — Access & Scoping Interview

One question at a time; skip anything already answered.

1. **Access type** — `Public GitHub repo` / `Private GitHub repo (requires gh auth already set up)` / `Local filesystem path` / `Not directly accessible — user will paste key files/docs` *(default: infer from what was given in Step 0)*
2. **Depth** — `Quick orientation` (idea + high-level architecture only) / `Deep dive` *(default — adds implementation details, dependencies, and health signals)*
3. **Focus areas** (multi-select, default: all) — `Product idea & positioning`, `Architecture & data flow`, `Implementation details & patterns`, `Dependencies & why`, `Health/maintenance signals` (skip this one by default for internal repos unless asked — see Guidelines)
4. **Why** — `Evaluating as a fork candidate`, `Learning from prior art / architecture inspiration`, `Onboarding to an internal codebase`, `Due diligence on a dependency` *(default: infer from context, else ask)* — shapes which findings get emphasized in the report

Announce which defaults were taken so the user can correct course.

### Step 2 — Gather Source Material

Follow [references/extraction-checklist.md](./references/extraction-checklist.md) for concrete lookup patterns per access type:

- **Public/private GitHub repo**: `WebFetch`/`gh` the README, manifest file(s) (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, etc.), top-level directory listing, `/docs` or `/adr` if present, `CONTRIBUTING.md`, `CHANGELOG.md`, and license file.
- **Local filesystem path**: use `Glob`/`Grep`/`Read` directly — same target list, read from disk instead of fetching.
- **Not directly accessible**: work only from what the user pastes; mark everything else as "not available — could not access."

**Evidence discipline**: every specific claim about structure, stack, or content must trace to something actually fetched or read this session. Never guess a dependency, a module's purpose, or a design pattern from a repo's name alone — mark anything unverifiable as "unverified."

### Step 3 — Extract the Idea & Positioning

From the README, docs, or (if linked and public) the project's landing page:

- One-line pitch and the problem it solves
- Target user / use case
- Positioning versus alternatives, if the README states it

If the repo doesn't state this explicitly, infer conservatively from the code/structure and label it clearly as **"inferred, not stated."**

### Step 4 — Map the Architecture

- Component/module map and data flow, concrete enough to sketch as short bullets or a simple text diagram
- Tech stack: languages, frameworks, datastore(s), deployment model
- Name the closest-matching pattern from [solution-candidate-design's architecture taxonomy](../solution-candidate-design/references/architecture-patterns.md) (monolith, modular monolith, Client+BaaS, serverless/event-driven, microservices, JAMstack, local-first, native mobile+thin backend) — note if it's a hybrid or doesn't cleanly fit any row

### Step 5 — Implementation Deep-Dive (Deep dive only)

- **Entry points** — where execution starts (`main.*`, `cmd/`, `src/index.*`, etc.)
- **Core modules/directories** and each one's responsibility, in plain language
- **Notable patterns or logic worth learning from** — anything a builder evaluating this repo would want to reuse or avoid re-inventing
- **Key dependencies and why** — not a full dependency list, just the load-bearing ones and what they're doing
- **Testing & CI approach** — what's actually tested, and how (framework, CI config if visible)
- **Extension points** — where a fork would plug in new functionality with the least friction

### Step 6 — Health & Risk Signals

- **License** — classify using [the feasibility skill's license guide](../solution-feasibility-study/references/license-guide.md); never guess from the repo name or README tone.
- **Maintenance activity** (external repos) — last commit date, release cadence, open issue/PR backlog, contributor count, all from actually fetched data (`gh api`/`WebFetch`) — never estimated.
- **Tech debt signals** — outdated/deprecated dependencies, missing or thin test coverage, TODO/FIXME density from a spot-check (state this is a spot-check, not an exhaustive audit).
- **Ownership/bus-factor signals** (internal repos, only if the focus area was selected) — keep this structural (e.g. "concentrated in one module, one recent contributor") rather than naming or evaluating individuals.

**Secrets handling**: if a fetched or read file appears to contain a credential, API key, or other secret, note *that* something sensitive exists and its location — never copy the actual secret value into the report or chat.

### Step 7 — Report & Present

Fill in [references/report-template.md](./references/report-template.md). Write it to `codebase-deep-dive-<repo-slug>.md` in the current working directory (confirm filename/location first if the directory isn't obviously a scratch/notes location). Present a condensed version in chat: idea summary, architecture sketch, top 3-5 implementation takeaways, and any license/health flags.

### Step 8 — Hand off

- **Evaluating as a fork candidate** → feed this report's findings into `solution-candidate-design`'s Fork & Extend candidate, or straight to `gh repo fork org/repo --clone` if the decision is already made.
- **Learned patterns to reuse when building fresh** → `repo-init`, pre-filling the stack/architecture answers this deep dive already confirmed.
- **Onboarding to an internal repo** → no handoff needed; the report itself is the onboarding artifact.
- **Found real security concerns, not just structural risk signals** → suggest `security-review` for a proper pass; this skill's health-signals step is not a substitute.

---

## Guidelines

- **Ground every claim.** Nothing about a repo's structure, stack, dependencies, or activity is stated unless it was actually fetched or read this session. Mark gaps as "unverified" or "not available" rather than filling them in from guesswork.
- **Distinguish stated from inferred.** Idea/positioning pulled from explicit README language versus inferred from code structure must be labeled differently.
- **One question at a time** during the interview.
- **Never paste secrets.** If a credential or key turns up in the source material, flag its existence and location only — never reproduce the value.
- **Respect access boundaries.** Don't attempt to bypass private-repo auth or scrape around rate limits. For internal repos, keep ownership/health notes structural, not a performance commentary on individuals.
- **Read-only.** This skill never modifies, forks, or clones the target repo on its own — it only suggests the next command for the user to run.
- **Not a security review.** Tech-debt and health signals here are orientation-level, not a vulnerability assessment — hand off to `security-review` if the user needs that.
- **Stay in scope.** This skill extracts and explains an existing project. It does not design new solution candidates (`solution-candidate-design`) or scaffold a new repo (`repo-init`) — hand off for those.
