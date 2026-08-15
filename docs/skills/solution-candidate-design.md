# solution-candidate-design — Solution Candidate Design

**Category**: Product Strategy &nbsp;|&nbsp; **Source**: [`skills/solution-candidate-design/SKILL.md`](../../skills/solution-candidate-design/SKILL.md)

## Purpose

Takes a validated (or validating) idea and produces 2-4 concrete, comparably-scoped **solution candidates** — fork-and-extend an existing GitHub repo, build from scratch with a specific architecture, or compose the product from existing managed services/APIs — each with a grounded architecture sketch, stack, effort estimate, and honest tradeoffs. Deliberately does **not** pick a winner: it lays candidates side by side so the user makes the call. Works standalone from just an idea/requirements, or chained after [solution-feasibility-study](solution-feasibility-study.md) and [audience-behavior-study](audience-behavior-study.md), reusing their fork candidates, technical-feasibility findings, and hard requirements.

## When to Use

- After a `solution-feasibility-study` Go/Conditional Go call, to turn its fork candidates and technical notes into real build options
- Jumping straight from an idea to architecture options without a prior study
- Deciding whether to fork an existing open-source project, build from scratch, or glue together managed services
- Wanting a side-by-side comparison instead of a single AI-picked "best" architecture

## Workflow

1. **Intake** — reads `feasibility-study-*.md` (fork candidates, technical feasibility, constraints) and `audience-study-*.md` (hard requirements implied by real pain points) from the working directory if present; otherwise asks for the idea and MVP scope directly.
2. **Scoping interview** — one question at a time: MVP scope, team & time constraints, technical comfort/stack preference, top priority (speed to MVP / scalability / cost / customization), openness to forking, and how many candidates to generate (2-4).
3. **Assemble the candidate pool** — builds one candidate per strong fork match, always at least one build-from-scratch candidate (picked from the [architecture patterns cheat sheet](../../skills/solution-candidate-design/references/architecture-patterns.md) to fit team size/budget/priority), and a compose-from-services candidate when the idea's core value isn't the infrastructure itself.
4. **Ground each candidate** — `WebFetch`es a fork's README/manifest to confirm its real stack rather than guessing, and runs targeted `WebSearch` queries on any hard technical dependency to sanity-check a from-scratch sketch.
5. **Candidate profiles** — architecture sketch, stack, effort estimate, pros/cons, and risks (technical, maintenance/license for forks, lock-in for composed services) for each candidate.
6. **Comparison matrix** — a Low/Med/High table across time-to-MVP, stack fit, scalability, maintenance burden, customization freedom, cost sensitivity, and key risk — with no declared winner. Close calls get a suggested spike/prototype instead of a guess.
7. **Report & hand off** — fills the [report template](../../skills/solution-candidate-design/references/report-template.md), writes `solution-candidates-<idea-slug>.md`, and once the user picks a direction points to `gh repo fork` + `ai-config` (fork chosen) or `repo-init` pre-filled with the chosen stack (build-from-scratch or compose chosen).

## Evidence Discipline

Every claim about a fork's stack/structure or an external service's capability must trace to something actually fetched or searched this session — never invented. Architecture pattern fit is drawn from the cheat sheet and adapted to the stated constraints, not assumed from familiarity.

## References

- [Architecture patterns cheat sheet](../../skills/solution-candidate-design/references/architecture-patterns.md) — pattern menu with fit/avoid guidance per team size and priority
- [Report template](../../skills/solution-candidate-design/references/report-template.md) — the structure filled in for the final report

## Related Skills

- [solution-feasibility-study](solution-feasibility-study.md) — run first for a Go/Conditional Go call and fork candidates this skill can reuse
- [audience-behavior-study](audience-behavior-study.md) — run first (or alongside) to ground candidates in real user requirements
- [codebase-deep-dive](codebase-deep-dive.md) — go deep on a specific Fork & Extend candidate before committing to it
- [repo-init](repo-init.md) — scaffold the repo once a build-from-scratch or compose candidate is chosen
- [ai-config](ai-config.md) — set up AI assistant configs once a fork or scaffolded repo exists
