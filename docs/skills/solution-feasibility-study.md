# solution-feasibility-study — Solution Feasibility Study

**Category**: Product Strategy &nbsp;|&nbsp; **Source**: [`skills/solution-feasibility-study/SKILL.md`](../../skills/solution-feasibility-study/SKILL.md)

## Purpose

Takes a software solution idea (SaaS side project, internal tool, startup concept) from a one-line pitch to a grounded build/pivot/no-build decision. Interviews the user to scope the research, then runs real research across market demand, commercial competitors, and the open-source/GitHub landscape, assesses technical feasibility and monetization, and produces a structured feasibility report — including any permissively-licensed GitHub repos worth forking instead of building from scratch.

## When to Use

- Evaluating a new SaaS or software product idea before committing time to it
- Researching commercial and open-source competitors for a concept
- Checking whether an existing open-source project could be forked as a starting point instead of building from zero
- Sanity-checking technical feasibility, monetization, or regulatory exposure for a solo/small-team build

## Workflow

1. **Intake** — capture the idea in the user's own words.
2. **Scoping interview** — one question at a time: target user, problem/urgency, team & time constraints, technical comfort, business model intent, research depth (deep dive vs. quick gut-check), and priority research areas.
3. **Research** — executes the [research playbook](../../skills/solution-feasibility-study/references/research-playbook.md) via web search and page fetches:
   - Market & demand signals
   - Commercial competitor landscape (features, pricing, weaknesses)
   - **GitHub / open-source landscape** — finds candidate repos, extracts stars/activity/license, and classifies each as a fork candidate using the [license guide](../../skills/solution-feasibility-study/references/license-guide.md)
   - Technical feasibility (matched against the user's stated stack/skill/time budget)
   - Monetization comparables
   - Legal/regulatory/trademark flags
   - Go-to-market channels realistic for the stated team size
4. **Synthesis & scoring** — scores market demand, differentiation, technical feasibility, monetization potential, competitive intensity, and regulatory risk (1-5 each), then rolls up to one call: Strong Go / Conditional Go / Pivot Suggested / No-Go.
5. **Report** — fills the [report template](../../skills/solution-feasibility-study/references/report-template.md), writes `feasibility-study-<idea-slug>.md` to the working directory, and presents a condensed summary in chat.

### Fork Candidates

Every permissively-licensed GitHub repo found during research is surfaced explicitly, with stars, last activity, license, why it fits, and what's missing — framed as a head start (`gh repo fork`) rather than a finished product. Copyleft or dual-licensed repos are flagged with the specific obligation they carry (e.g., AGPL's network-use clause), not just the license name. All license classifications carry an explicit "not legal advice, verify before shipping commercially" disclaimer.

## Evidence Discipline

Every claim (pricing, star counts, license, activity dates) must trace to something actually fetched during the session — nothing is invented. Failed lookups are marked "unverified" rather than guessed.

## References

- [Research playbook](../../skills/solution-feasibility-study/references/research-playbook.md) — concrete search/fetch patterns per research area
- [License guide](../../skills/solution-feasibility-study/references/license-guide.md) — permissive vs. copyleft vs. unclear classification for fork candidates
- [Report template](../../skills/solution-feasibility-study/references/report-template.md) — the structure filled in for the final report

## Related Skills

- [project-idea-brainstorm](project-idea-brainstorm.md) — find an idea to feed into this skill if you don't have one yet
- [repo-init](repo-init.md) — scaffold a new repository once a build decision is made
- [ai-config](ai-config.md) — set up AI assistant configs after choosing to build (or fork) a project
