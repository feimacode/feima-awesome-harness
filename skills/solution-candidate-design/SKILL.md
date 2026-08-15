---
name: solution-candidate-design
description: "Turn a validated idea into 2-4 concrete solution candidates for the user to manually compare — fork-and-extend (using GitHub repos surfaced by solution-feasibility-study), build-from-scratch, and compose-from-existing-services, each with a grounded architecture sketch, stack, effort estimate, risks, and tradeoffs, plus a side-by-side comparison matrix. Deliberately does not pick a winner — hands the decision to the user. Works standalone (jump straight from an idea/requirements to architecture options) or chained after solution-feasibility-study / audience-behavior-study, reusing their fork candidates and constraints. Triggers on: 'design solution options', 'compare architecture candidates', 'should I fork or build this', 'give me some architecture options', 'solution architecture', 'help me pick a tech approach', 'what are my build options'."
argument-hint: "Optional: paste the idea/requirements directly, or point at a feasibility-study-<slug>.md file to skip the intro question"
---

# Solution Candidate Design

## Purpose

Take a validated (or validating) idea and produce a small set of concrete, comparably-scoped **solution candidates** — not one recommendation. Each candidate is a real option: fork and extend an existing open-source repo, build from scratch with a specific architecture, or compose the product from existing managed services/APIs. Each gets a grounded architecture sketch, stack, effort estimate, and honest tradeoffs, laid out side by side so the user can make the call themselves.

This is the natural step after [`solution-feasibility-study`](../solution-feasibility-study/SKILL.md) (and optionally [`audience-behavior-study`](../audience-behavior-study/SKILL.md)) — feasibility asks "should we build this," audience research asks "who for and how do we talk to them," this skill asks "what are the concrete ways we could actually build it." It also works as a direct entry point: a user with an idea and no prior study can jump straight here.

This skill designs and compares candidates. It does not pick a winner, does not write production code, and does not scaffold a repository — hand off to `repo-init` (and `ai-config`) once the user has chosen a direction.

## Platform Adaptation

Before starting the interview, load the reference file for the current agent environment and follow its tool guidance (shared with `repo-init`, `solution-feasibility-study`, and `audience-behavior-study`):

- [Copilot CLI mapping](../repo-init/references/copilot-tools.md)
- [Codex mapping](../repo-init/references/codex-tools.md)
- [Gemini CLI mapping](../repo-init/references/gemini-tools.md)

Prefer the platform's native structured question/input tool over plain chat whenever one exists.

---

## Procedure

### Step 0 — Intake

Look for a `feasibility-study-*.md` file in the current working directory (or one the user names/pastes). If found, read it and reuse:

- The idea summary and target user
- Section 4.2/4.3 (Open-Source/GitHub Landscape, Build vs. Fork vs. Buy) — the **Forkable** and **Caution** repos become fork-candidate seeds for this skill
- Section 6 (Technical Feasibility) — the hardest dependency and stated stack comfort
- Section 7 (Monetization) and team/time constraints implied by the report

Also check for an `audience-study-*.md` file — if present, pull any pain points or jobs-to-be-done that imply a hard technical requirement (e.g. real-time collaboration, offline support) so candidates don't miss it.

Confirm in one line what was reused. If neither file exists, ask directly:

> "What's the idea, and what does it need to do at minimum (a short MVP feature list)? Also — any GitHub repos already on your radar as a possible starting point?"

### Step 1 — Scoping Interview

One question at a time; skip anything already answered by a prior report. Every question has a marked default.

1. **MVP scope** — the short list of must-have features/capabilities *(default: infer from the idea description, confirm)*
2. **Team & time constraints** — solo/small team/funded team, and rough time budget *(default: reuse from feasibility report, else solo/part-time)*
3. **Technical comfort / stack preference** — languages/frameworks already comfortable with, or "no preference" *(default: reuse from feasibility report, else infer from any repo context in the current working directory)*
4. **Top priority** — `Speed to MVP` *(default)*, `Long-term scalability`, `Lowest ongoing cost`, `Maximum customization/control`
5. **Openness to forking** — `Open to forking if a good match exists` *(default)*, `Prefer building from scratch`, `Prefer composing from managed services/APIs where possible`
6. **How many candidates** — *(default: 3)*, min 2 max 4

Announce which defaults were taken so the user can correct course.

### Step 2 — Assemble the Candidate Pool

Build a pool covering distinct *approaches*, not variations on one theme:

- **Fork & Extend** — one candidate per serious fork-candidate repo (from Step 0, or freshly found via `WebSearch`/`WebFetch` if none were provided and the user is open to forking). Cap at the 1-2 strongest matches by feature overlap; don't pad the pool with weak fits.
- **Build from Scratch** — always include at least one. Pick an architecture pattern from [references/architecture-patterns.md](./references/architecture-patterns.md) that matches the team size, time budget, and priority from Step 1 — don't default to microservices for a solo weekend builder or to a single monolith for a spec that clearly needs independent scaling.
- **Compose from Existing Services** — include when the idea's core value isn't the infrastructure itself (e.g. it's a workflow/UX layer over commodity capability like auth, payments, storage, email, or a hosted LLM). Sketch it as gluing managed services/APIs together rather than building that capability in-house.

Skip a category only if it's a genuinely poor fit (e.g. no forkable repo exists and the user isn't open to forking) — say so explicitly rather than silently omitting it.

### Step 3 — Ground Each Candidate

**Fork & Extend candidates**: `WebFetch` the repo's README, and its manifest file if visible (`package.json`, `pyproject.toml`, `go.mod`, etc.) to confirm the actual stack and folder/module structure — do not guess architecture from the repo name or description. Note what the fork already covers versus what the idea still needs built on top. For a strong fork candidate the user wants to go deeper on before committing, suggest `codebase-deep-dive` for a full implementation/health-signal pass instead of this skill's lighter README-level check.

**Build from Scratch candidates**: use [references/architecture-patterns.md](./references/architecture-patterns.md) as the base pattern, then run 1-2 `WebSearch` queries on the hardest technical dependency identified in Step 0/1 (e.g. `"<hard dependency>" architecture` or `"<hard dependency>" how to build`) to sanity-check the sketch against how comparable products are actually built — reuse this from a feasibility report's Section 6 if it already did this research.

**Compose from Services candidates**: `WebSearch` for the realistic managed-service building blocks (e.g. `<capability> managed API`, `<capability> BaaS`) and note the ones that best fit the stated stack comfort and budget sensitivity.

**Evidence discipline**: every specific claim about a fork's stack, structure, or an external service's capability must trace to something actually fetched or searched this session. Mark anything unverifiable as "unverified" — never invent a repo's tech stack, a service's pricing tier, or an architecture pattern's fit.

### Step 4 — Candidate Profiles

For each candidate, produce:

- **Name/label** and one-line description
- **Starting point** — `Fork of org/repo` / `Build from scratch` / `Compose from services`
- **Architecture sketch** — components and data flow, described concretely enough to picture (short bullets or a simple text diagram; a full design doc is out of scope here)
- **Stack** — languages/frameworks/infra, matched against the user's stated comfort
- **Effort estimate** — time bucket (`Weekend prototype` / `A few months part-time` / `6-12 months` / `Needs a team`), matched against the stated time budget
- **Pros**
- **Cons**
- **Risks** — technical risk, maintenance/dependency risk (for forks: upstream drift, license obligations per [the feasibility skill's license guide](../solution-feasibility-study/references/license-guide.md)), lock-in risk (for composed services)

### Step 5 — Comparison Matrix (no winner declared)

Lay candidates side by side on consistent dimensions, each rated Low/Med/High with a one-line rationale — **not** a single weighted score and **not** a stated "best" option:

| Dimension | Candidate A | Candidate B | Candidate C |
|---|---|---|---|
| Time to MVP | | | |
| Stack fit (vs. stated comfort) | | | |
| Long-term scalability | | | |
| Ongoing maintenance burden | | | |
| Customization freedom | | | |
| Cost sensitivity | | | |
| Key risk | | | |

If two candidates are close on the dimensions the user said matter most (Step 1, priority), say so explicitly and suggest a short spike/prototype on the specific uncertainty rather than guessing which one is "better."

### Step 6 — Report & Present

Fill in [references/report-template.md](./references/report-template.md). Write it to `solution-candidates-<idea-slug>.md` in the current working directory (confirm filename/location first if the directory isn't obviously a scratch/notes location). Present the candidate profiles and comparison matrix in chat, then explicitly ask the user which direction they want to take — do not recommend one yourself unless directly asked, and if asked, frame it as your read of *their* stated priority, not an objective ranking.

### Step 7 — Hand off

Once the user picks (or narrows to a couple to prototype):

- **Fork & Extend chosen** → suggest `gh repo fork org/repo --clone` (or `git clone` + detach from upstream), then `ai-config` to set up AI assistant configs on top of the fork.
- **Build from Scratch chosen** → suggest `repo-init`, and pre-fill what's already known (language/runtime, stack pieces, database, delivery target) so the wizard's Section 2 goes faster.
- **Compose from Services chosen** → suggest `repo-init` for the thin app layer, noting which pieces are external services rather than local scaffolding.

---

## Guidelines

- **Never declare a winner.** This skill's entire value is putting real, grounded options in front of the user for *their* judgment call — a forced single recommendation defeats the purpose. State tradeoffs, not verdicts.
- **One question at a time** during the interview.
- **Confirm inferred defaults**, especially when reusing a prior feasibility/audience report.
- **Ground every architecture claim.** Don't invent a fork's tech stack, an external service's capability, or an architecture pattern's suitability — fetch/search it, or mark it unverified.
- **Comparable scope across candidates.** Don't sketch one candidate in detail and hand-wave another — if time is short, trim the candidate pool (Step 1, question 6) rather than the depth per candidate.
- **Respect stated constraints.** Architecture complexity and stack picks must match the team size and time budget from Step 1 — don't propose a microservices build for "solo, weekends" or a fragile no-code stack for "funded team, needs to scale."
- **Stay in scope.** This skill designs and compares candidates. It does not write product code or scaffold a repo — hand off to `repo-init`/`ai-config` once a direction is chosen.
