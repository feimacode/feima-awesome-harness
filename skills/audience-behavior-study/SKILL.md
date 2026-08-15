---
name: audience-behavior-study
description: "Deep audience and user-behavior research for a software idea's target users — mines real public conversations on Reddit, X/Twitter, review sites, and niche communities for pain points, current workarounds, objections, willingness-to-pay signals, and the vocabulary users actually use. Interviews the user briefly (or reads a prior solution-feasibility-study report for context), runs the research, then produces grounded personas, verbatim evidence, a messaging-language glossary, and a channel map. Natural next step after solution-feasibility-study gives a Go/Conditional Go call. Triggers on: 'audience research', 'user behavior study', 'who are my users', 'find user pain points on reddit/x', 'target audience research', 'persona research', 'messaging research', 'find out what my users are saying online'."
argument-hint: "Optional: paste the idea/ICP directly, or point at a feasibility-study-<slug>.md file to skip the intro question"
---

# Audience & User Behavior Study

## Purpose

Take an idea's target user from a one-line description to grounded, evidence-backed audience understanding: who they are, what they complain about today, what language they use to describe the problem, what they've already tried, what would make them pay, and where they actually hang out online. This skill mines real public conversations — Reddit, X/Twitter, review sites, niche communities — rather than inventing personas from imagination.

It is the natural next step once [`solution-feasibility-study`](../solution-feasibility-study/SKILL.md) has produced a Go/Conditional Go call: feasibility asks "should we build this," this skill asks "who exactly are we building for, and how do we talk to them so it lands."

This skill researches public audience behavior and produces messaging/persona material. It does not run live surveys or interviews with real people, does not replace user interviews once actual users exist, and does not write GTM/marketing copy itself — it hands back the raw material for that.

## Platform Adaptation

Before starting the interview, load the reference file for the current agent environment and follow its tool guidance (shared with `repo-init` and `solution-feasibility-study`):

- [Copilot CLI mapping](../repo-init/references/copilot-tools.md)
- [Codex mapping](../repo-init/references/codex-tools.md)
- [Gemini CLI mapping](../repo-init/references/gemini-tools.md)

Prefer the platform's native structured question/input tool over plain chat whenever one exists.

---

## Procedure

### Step 0 — Intake

Look for a `feasibility-study-*.md` file in the current working directory (or one the user names/pastes). If found, read it and pull the idea summary, target user/ICP, and any market-signal findings from Section 2/3 so the user doesn't repeat themselves — confirm what was reused in one line.

Otherwise ask:

> "What's the idea, and who's the target user? A couple of sentences is fine."

### Step 1 — Scoping Interview

One question at a time; every question has a marked default.

1. **Target segment(s)** — one ICP or several distinct ones to study separately? *(default: the single ICP from the feasibility report or idea description)*
2. **Communities/platforms to prioritize** — `Reddit`, `X/Twitter`, `Hacker News`, `Niche forums/Discord/Slack`, `Review sites (G2/Capterra/App Store)` *(default: Reddit + X, add review sites if competitor products already exist from the feasibility study)*
3. **Research goal** — `Positioning & messaging language` *(default)*, `Persona / jobs-to-be-done depth`, `Channel & GTM mapping`, `All of the above`
4. **Research depth** — `Quick scan` *(default — one pass per theme, ~15-20 searches)*, `Deep dive` (more threads/quotes per theme, multiple sub-segments)

Announce which defaults were taken so the user can correct course.

### Step 2 — Research

Run the playbook at [references/research-playbook.md](./references/research-playbook.md), covering:

- Pain-point mining (verbatim quotes, not paraphrases-of-paraphrases)
- Current workarounds / tools already being cobbled together
- Objections & skepticism ("I tried X and it didn't work because...")
- Willingness-to-pay signals
- Vocabulary/language glossary — the words users use for the problem, versus internal or marketing jargon
- Review-site mining for existing competitor products, if any were found in a prior feasibility study
- Community/channel map — which subreddits, X communities/hashtags, Discord/Slack servers, or forums are actually active for this audience, with a rough activity signal

**Evidence discipline** (same standard as `solution-feasibility-study`):
- Every quote, claim, or signal must trace to something actually fetched this session. Cite the source (link) next to it.
- If a search or fetch fails, write "unverified" and move on — never invent a quote, thread, handle, or engagement number.
- Prefer the original thread/post over a secondhand summary.

**Public/aggregate line — do not cross it**:
- Only surface content that is public and quote or paraphrase it with a link back to the source.
- Do not collect or output personal identifying information beyond a public handle/username already visible in the post.
- Never suggest contacting, DMing, or targeting specific individuals found during research — this is listening research, not outreach or lead generation.
- Never suggest scraping techniques that bypass a platform's ToS or rate limits — stick to `WebSearch`/`WebFetch` at normal, snippet-level volume.
- If a search turns up a private or clearly non-public group (a closed Discord, a gated forum), do not attempt to access it — note the community exists and move on.

### Step 3 — Synthesis

Cluster findings into 2-4 personas (fewer if the user chose a single segment). For each persona:

- **Who they are** — role/context, inferred from the research, not stereotyped
- **Jobs-to-be-done** — what they're actually trying to accomplish
- **Top pain points** — each backed by at least one verbatim quote + link
- **Current workarounds** — what they use/do today instead
- **Objections** — reasons they might not adopt a new solution, grounded in real complaints found
- **Willingness-to-pay signal** — strong/weak/none, with the evidence that led to that call
- **Language glossary** — 5-10 phrases this persona actually uses for the problem, for later use in copy

Then build a **channel map**: platform/community, activity level, what content resonates there, and the realistic way to reach this audience (organic content, community participation, paid, etc.) — matched against a solo/small-team builder's capacity unless the feasibility report said otherwise.

### Step 4 — Report

Fill in [references/report-template.md](./references/report-template.md). Write it to `audience-study-<idea-slug>.md` in the current working directory (confirm filename/location first if the directory isn't obviously a scratch/notes location). Present a condensed version in chat: persona summary, top 3-5 pain-point quotes, language glossary highlights, and the channel map.

### Step 5 — Hand off

Point the user at how to use the output rather than doing it here: feeding the language glossary and pain-point quotes into landing-page/marketing copy, refining the go-to-market section of an existing `feasibility-study-*.md` if one exists, moving to `solution-candidate-design` to turn the idea into concrete, comparable build options (the pain points here can flag hard requirements a candidate architecture must satisfy — e.g. real-time collaboration), or — once real users exist — running actual user interviews to validate what this research only infers from public behavior.

---

## Guidelines

- **One question at a time** during the interview.
- **Confirm inferred defaults**, especially when reusing a prior feasibility report's ICP.
- **Don't fabricate quotes, threads, handles, or engagement numbers.** Mark anything unverifiable as such.
- **Public and aggregate only.** No individual targeting, no outreach suggestions, no scraping that bypasses ToS/rate limits, no collecting personal info beyond what's already publicly visible in a quoted post.
- **Informal signal, not survey-grade research.** State plainly that this is a public-behavior listening pass — real user interviews are still the gold standard once the user has actual users to talk to.
- **Stay in scope.** This skill produces audience-research material — personas, quotes, language, channel map. It does not write marketing copy, build a GTM plan, run outreach, or design the solution itself; hand off to `solution-candidate-design` for build options once audience research is done.
