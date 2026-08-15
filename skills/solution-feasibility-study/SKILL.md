---
name: solution-feasibility-study
description: "Guided feasibility study for a software solution idea (SaaS side project, internal tool, startup concept). Interviews the user about the idea, target users, and constraints, then researches market demand, commercial competitors, and the open-source/GitHub landscape, assesses technical feasibility and monetization, and produces a structured feasibility report with a go/no-go recommendation. Surfaces permissively-licensed GitHub repos as fork candidates so the user can start from existing code instead of building from scratch. Triggers on: 'evaluate this idea', 'is this a good SaaS idea', 'feasibility study', 'should I build this', 'validate my side project idea', 'competitor research', 'find competitors on GitHub', 'is there an open source alternative I could fork'."
argument-hint: "Optional: paste your idea directly to skip the intro question"
---

# Solution Feasibility Study

## Purpose

Take a software solution idea from a one-line pitch to a grounded build/pivot/no-build decision. Interview the user just enough to scope the research, then run real research (web + GitHub) across market, competitive, technical, and monetization dimensions, and hand back a structured report — including any existing open-source projects permissively licensed enough to fork instead of building from zero.

This skill evaluates and researches. It does not write the product's code, and it does not replace legal or financial advice.

## Platform Adaptation

Before starting the interview, load the reference file for the current agent environment and follow its tool guidance (shared with `repo-init`):

- [Copilot CLI mapping](../repo-init/references/copilot-tools.md)
- [Codex mapping](../repo-init/references/codex-tools.md)
- [Gemini CLI mapping](../repo-init/references/gemini-tools.md)

Prefer the platform's native structured question/input tool over plain chat whenever one exists.

---

## Procedure

### Step 0 — Intake

If the user already pasted an idea description (via the argument or their message), skip straight to confirming it back to them in 1-2 sentences and move to Step 1. Otherwise ask:

> "What's the idea? A couple of sentences is fine — what does it do, and who is it for?"

### Step 1 — Scoping Interview

Ask the following, one at a time. Every question has a marked default; "skip" or a blank reply accepts it.

1. **Target user / ICP** — who has this problem today? *(default: infer from the idea description and confirm)*
2. **Problem & urgency** — what do they do today without this, and why would they switch? *(default: infer and confirm)*
3. **Team & constraints** — solo, small team, or funded team? *(default: solo side project)*
4. **Time budget** — weekend prototype, a few months of evenings/weekends, or full-time? *(default: a few months, part-time)*
5. **Technical comfort** — languages/stacks the builder is already comfortable with *(default: infer from any repo context in the current working directory, else ask)*
6. **Business model intent** — Options: `SaaS subscription` *(default)*, `One-time purchase`, `Open-source + paid support/hosting`, `Marketplace/commission`, `Not sure yet / ad-supported`, `Other`
7. **Research depth** — Options: `Deep dive` *(default — full pipeline below)*, `Quick gut-check` (market + competitors only, ~half the research volume, skip monetization/legal detail)
8. **Priority areas** (multi-select, default: all) — `Market & demand`, `Competitors & GitHub/OSS landscape`, `Technical feasibility`, `Monetization`, `Legal/regulatory`, `Go-to-market`

Announce which defaults were taken so the user can correct course. Preserve all answers — they drive both the research plan and the final report.

### Step 2 — Research

Run the research playbook at [references/research-playbook.md](./references/research-playbook.md), covering the areas selected in Step 1. It defines concrete WebSearch query patterns and WebFetch targets for:

- Market & demand signals
- Commercial competitor landscape
- **GitHub / open-source competitor research** — including how to find candidate repos, what to extract (stars, activity, license), and how to classify each as a fork candidate
- Technical feasibility signals
- Monetization comparables
- Legal / regulatory / trademark flags
- Go-to-market channels realistic for the stated team size

For license classification specifically, use [references/license-guide.md](./references/license-guide.md) — do not guess a license from a repo's name or description; only classify from the license actually reported on the repo (GitHub's detected-license metadata or a fetched `LICENSE` file).

**Time-box**: "Quick gut-check" = top 3-5 competitors and top 3-5 GitHub repos. "Deep dive" = top 5-8 of each, plus monetization/legal/go-to-market detail.

**Evidence discipline**:
- Every factual claim (a competitor's pricing, a repo's star count, a market signal) must trace to something actually fetched during this session. Cite the source (link) next to the claim.
- If a lookup fails or a page can't be fetched, write "unverified" and move on — never invent numbers, star counts, license names, or activity dates.
- Prefer primary sources (the repo itself, the competitor's own pricing page) over secondhand summaries.

### Step 3 — Synthesis & Scoring

Score each researched dimension 1-5 (5 = strongly favorable) with one line of rationale per score, using the findings from Step 2:

| Dimension | What favors a high score |
|---|---|
| Market demand | Active, recurring pain with evidence of people paying or actively seeking solutions |
| Differentiation | Clear gap versus commercial competitors and OSS alternatives found |
| Technical feasibility | Buildable by the stated team/skill/time budget without novel R&D |
| Monetization potential | Plausible path to revenue that matches the stated business model intent |
| Competitive intensity | Score high when the space is *not* saturated/commoditized |
| Regulatory/legal risk | Score high when there are *few* compliance or trademark obstacles |

Combine into one overall call — **Strong Go / Conditional Go / Pivot Suggested / No-Go** — and state the one or two factors that most drove the call. Be balanced: a feasibility study earns its keep by surfacing real reasons *not* to build something, not by cheerleading.

### Step 4 — Report

Fill in [references/report-template.md](./references/report-template.md) with the interview answers and research findings. Write it to `feasibility-study-<idea-slug>.md` in the current working directory (confirm the filename/location with the user first if the directory isn't obviously a scratch/notes location). Then present a condensed version of the same structure in chat: idea summary, overall recommendation, top 3 competitors, top fork candidate (if any), and next steps.

#### Fork Candidates

If Step 2 found any GitHub repos classified as **Forkable** (permissive license, see [references/license-guide.md](./references/license-guide.md)), give them their own visible callout in both the report and the chat summary — this is a primary offering of this skill, not a footnote:

```
## Fork Candidates

| Repo | Stars | Last Activity | License | Why it fits |
|---|---|---|---|---|
| org/name | 1.2k | 2026-07 | MIT | Covers ~70% of the core feature set; active maintenance |

**Suggested next step:** `gh repo fork org/name --clone` (or `git clone` + detach from upstream) to use as a starting point instead of building from zero.
```

For each fork candidate, note *why* it's a fit (feature overlap with the idea, maintenance health) and *what's missing* (the gap the user would still need to build) so forking is framed as a head start, not a finished product. If the user wants a full implementation-level look at a specific fork candidate before committing, suggest `codebase-deep-dive`.

#### Handoff

If the overall call is **Strong Go** or **Conditional Go**, suggest `audience-behavior-study` as the natural next step (e.g. "run `/audience-behavior-study` to go deep on how this target user actually behaves online — pain points, language, and channels — before you start building or writing copy"). It can reuse this report's idea summary and target user instead of re-asking. If the user wants to skip straight to build options, `solution-candidate-design` can also reuse this report's fork candidates and technical-feasibility findings directly.

---

## Guidelines

- **One question at a time** during the interview; don't dump the whole Step 1 list in one message.
- **Confirm inferred defaults.** If you inferred the target user or tech stack instead of asking, say so plainly before moving on.
- **License claims are not legal advice.** Always include this disclaimer in the report's Fork Candidates section: license classification here is a starting point, not legal counsel — the user should verify the actual `LICENSE` file (and check for trademark/branding restrictions, dual licensing, or CLA requirements) before shipping anything derived from a fork commercially.
- **Don't fabricate research.** No invented star counts, pricing, license names, or dates. Mark anything unverifiable as such rather than smoothing it over.
- **Stay in scope.** This skill produces a decision-support report, not product code, not a full business plan, not a legal opinion. If the user wants to proceed to building, hand off — e.g. suggest `solution-candidate-design` to turn the fork candidates and technical-feasibility findings into concrete, comparable build options, or `repo-init` directly if the user already knows their direction. If the call is Go/Conditional Go, also suggest `audience-behavior-study` for a deeper pass on target-user pain points, language, and channels before building or writing copy.
- **Respect the stated constraints.** Technical feasibility and go-to-market recommendations should match the team size and time budget from Step 1 — don't recommend a GTM motion or architecture that assumes a funded team when the user said "solo, weekends."
