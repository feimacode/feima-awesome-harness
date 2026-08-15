# codebase-deep-dive — Codebase Deep Dive

**Category**: Codebase Research &nbsp;|&nbsp; **Source**: [`skills/codebase-deep-dive/SKILL.md`](../../skills/codebase-deep-dive/SKILL.md)

## Purpose

Takes an existing project — a public GitHub repo, a private repo, or an internal/local repo — and extracts a structured, grounded brief: what it does and for whom, its architecture and tech stack, how it's actually built (entry points, key modules, notable patterns, dependencies and why they're there), and health/risk signals (maintenance activity, license, tech debt, ownership concentration). It's a research/extraction skill, not a code audit or security review — for that, use `security-review`.

Useful standalone (point it at any repo you want to understand) or chained after [solution-feasibility-study](solution-feasibility-study.md) or [solution-candidate-design](solution-candidate-design.md), to go deep on one specific fork candidate instead of the lighter README-level pass those skills do.

## When to Use

- Sizing up a GitHub repo as a fork candidate before committing to it
- Learning from prior art / borrowing architecture patterns for a new build
- Onboarding to an internal codebase you didn't write
- Due diligence on a dependency or vendored project
- Wanting a real understanding of a repo's implementation, not just its README

## Workflow

1. **Intake** — reuses a named fork candidate from a `feasibility-study-*.md`/`solution-candidates-*.md` file, or a GitHub URL/local path pasted directly; otherwise asks which repo.
2. **Access & scoping interview** — access type (public/private GitHub, local path, or paste-only if inaccessible), depth (quick orientation vs. deep dive), focus areas, and why (fork evaluation, learning, onboarding, or dependency due diligence).
3. **Gather source material** — follows the [extraction checklist](../../skills/codebase-deep-dive/references/extraction-checklist.md): README, manifest files, directory layout, docs/ADRs, CI config, and license file, fetched via `WebFetch`/`gh` for GitHub or read directly via `Glob`/`Grep`/`Read` for local paths.
4. **Extract idea & positioning** — pitch, target user, and positioning, labeled as stated-in-README vs. inferred from code.
5. **Map the architecture** — component/data-flow sketch, stack, and the closest-matching pattern from `solution-candidate-design`'s architecture taxonomy.
6. **Implementation deep-dive** (deep dive only) — entry points, core modules, notable patterns worth learning from, key dependencies and why, testing/CI approach, extension points.
7. **Health & risk signals** — license classification, maintenance activity (external repos), tech-debt spot-check, structural ownership notes (internal repos), and a secrets check that flags location only, never reproduces values.
8. **Report** — fills the [report template](../../skills/codebase-deep-dive/references/report-template.md), writes `codebase-deep-dive-<repo-slug>.md`, and presents a condensed summary in chat.
9. **Hand off** — into `solution-candidate-design`'s Fork & Extend candidate, `repo-init` (pre-filled from confirmed stack), or `security-review` if real security concerns turn up.

## Evidence Discipline & Boundaries

Every claim about structure, stack, dependencies, or activity must trace to something actually fetched or read this session — otherwise it's marked "unverified" or "not available," never guessed. Read-only: never modifies, forks, or clones the target repo on its own. Never reproduces secret values found in source material — flags existence and location only. For internal repos, ownership/health notes stay structural, not commentary on individuals.

## References

- [Extraction checklist](../../skills/codebase-deep-dive/references/extraction-checklist.md) — concrete lookup patterns per access type
- [Report template](../../skills/codebase-deep-dive/references/report-template.md) — the structure filled in for the final report
- [License guide](../../skills/solution-feasibility-study/references/license-guide.md) (shared with `solution-feasibility-study`) — license classification, not legal advice
- [Architecture patterns](../../skills/solution-candidate-design/references/architecture-patterns.md) (shared with `solution-candidate-design`) — pattern taxonomy used to name the extracted architecture

## Related Skills

- [solution-feasibility-study](solution-feasibility-study.md) — surfaces fork-candidate repos this skill can go deep on
- [solution-candidate-design](solution-candidate-design.md) — feed this skill's findings into a grounded Fork & Extend candidate
- [repo-init](repo-init.md) — scaffold a fresh repo using patterns learned here
- `security-review` (built-in Claude Code skill, not part of this repo) — for an actual vulnerability assessment, beyond this skill's orientation-level health signals
