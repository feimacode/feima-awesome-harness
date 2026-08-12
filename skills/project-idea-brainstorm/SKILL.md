---
name: project-idea-brainstorm
description: "Guided brainstorming session that helps founders/creators find project ideas to build — side projects, SaaS, open-source tools, internal tools, or startup concepts. Asks the user for a few kick-off topics (or offers a menu of popular topics, e.g. AI-related, if they have none), optionally digs into interested domains, past projects, and builder constraints, then produces a diverse shortlist of specific, hooked idea pitches. Brainstorming only — does not research competitors, market size, or feasibility. Explicitly hands off validated-sounding ideas to the solution-feasibility-study skill. Triggers on: 'brainstorm project ideas', 'help me find something to build', 'what should I build next', 'give me some startup ideas', 'side project ideas', 'I want to build something but don't know what'."
argument-hint: "Optional: list a few topics/domains directly to skip the intro question"
---

# Project Idea Brainstorm

## Purpose

Help a founder/creator go from "I want to build something" to a shortlist of concrete, specific project ideas worth considering. This skill generates and diversifies ideas — it does not validate them. Any idea the user wants to pursue further should be handed to `solution-feasibility-study` for market/competitor/technical/monetization research and a go/no-go call.

## Procedure

### Step 0 — Intake: kick-off topics (required)

Brainstorming needs an anchor — generating ideas from nothing produces generic noise. If the user already gave topics (via the argument or their message), confirm them back in one line and move to Step 1.

Otherwise ask:

> "What are a few topics, domains, or interests you'd like to build around? Pick a couple, or say 'suggest some' and I'll offer some popular starting points (including AI-related ones)."

If the user has nothing in mind or asks for suggestions, present the **Popular Topics Menu** below (via `AskUserQuestion` if available, multi-select) so they can pick instead of staring at a blank page.

#### Popular Topics Menu

A living set of evergreen-plus-current buckets — refresh the "current" half with a quick `WebSearch` (e.g. `trending startup ideas <current year>`, `Y Combinator request for startups <current year>`, `indie hacker trending ideas <current year>`) when the user wants genuinely current suggestions rather than relying on memory:

- **AI agents & agentic tooling** — coding agents, workflow automation, browser/computer-use agents
- **AI in a specific vertical** — legal, healthcare, education, real estate, trades/field service
- **Realtime & voice AI** — oral practice, meeting assistants, live transcription/translation
- **Developer tools & infra** — observability, local-first software, dev experience, DX for AI-era workflows
- **Personal knowledge tools** — note-taking, "second brain," research assistants
- **Climate & sustainability tech**
- **Creator economy & content tooling**
- **Fintech & personal finance automation**
- **Health & wellness tracking**
- **Community/social software for niche groups**

Treat this list as a starting point, not a fixed catalog — swap in genuinely current items when a grounding search turns up something fresher.

### Step 1 — Lightweight scoping interview

One question at a time, defaults let the user skip fast. Skip anything already answered or clearly irrelevant given the kick-off topics:

1. **Interested domains beyond the kick-off topics** — anything else to lean into, or explicitly avoid? *(default: use kick-off topics as-is)*
2. **Historical projects** — what have they built before (side projects, work projects, abandoned ideas)? Useful for surfacing reusable skills/code or "sequel" ideas. *(default: skip, treat as a blank slate)*
3. **Builder profile** — solo or team, stack/technical comfort, rough time budget. *(default: infer from any repo context in the current working directory, else assume solo/part-time)*
4. **Idea shape preference** — `SaaS product`, `Open-source / dev tool`, `Internal/personal tool`, `Content/media project`, or `No preference` *(default: No preference)*
5. **Risk appetite** — `Safe & buildable` (close to existing skills, short build time), `Mix of safe and ambitious` *(default)*, `Swing big` (bigger bets, more unknowns okay)

Announce which defaults were taken so the user can correct course.

### Step 2 — Scan founder chatter on X and Reddit

Before generating ideas, spend a handful of searches listening to what other founders/builders are already saying about the kick-off topics — other people's live discussions are a sharper idea source than pure LLM imagination, and they double as an early demand signal.

- `WebSearch`: `site:reddit.com <topic> "side project" OR "what should I build" OR "anyone working on" OR "someone should build"`
- `WebSearch`: `site:reddit.com <topic> (r/SideProject OR r/startups OR r/Entrepreneur OR r/SaaS OR r/indiehackers)`
- `WebSearch`: `site:x.com OR site:twitter.com <topic> "building" OR "idea" OR "someone should build this"`
- `WebSearch`: `<topic> indie hackers idea` / `<topic> build in public`

For each useful thread/post found, note: what idea or pain point was raised, how it landed (reply count, agreement, "I'd pay for this"-type reactions — treated as an informal signal, not verified demand), and whether anyone mentioned already building it. `WebFetch` a thread if the search snippet is too thin to judge.

Fold what you find into Step 3 two ways: (a) let a real founder pain point directly seed or sharpen one of the generated ideas — cite the thread inline when a pitch traces back to one; (b) if the same request/complaint shows up repeatedly across threads, call that out explicitly as a stronger-than-usual signal.

Keep this to a handful of searches per session — it's a listening pass to sharpen and ground the ideas, not the deep market/competitor research `solution-feasibility-study` performs later.

### Step 3 — Generate the idea set

Produce 8-12 candidate ideas (fewer if the user asked for something narrower or picked `Safe & buildable`). For **each** idea give:

- **One-line pitch**
- **Why it fits** — ties back to their stated topics, domain interests, history, or skills
- **Rough shape** — SaaS / OSS / internal tool / content, plus a rough size signal (weekend / a few months / bigger bet)
- **The hook** — the specific wedge, timing reason, or underserved angle that makes it non-generic

Diversify using multiple ideation techniques rather than variations on one theme — mix several of:

- **Cross a kick-off topic with an adjacent/trending bucket** (e.g. "language learning" × "realtime voice AI")
- **Old industry + AI** — a manual/analog process in a specific vertical that's newly tractable
- **Personal itch** — a workflow annoyance implied by their own stated history/stack
- **Recent capability unlock** — something newly possible because of a recent model/API/platform capability (worth 1-2 extra `WebSearch` queries if Step 2 didn't already surface one)
- **Underserved niche of a crowded category** — a specific segment the big players in that space ignore
- **A founder-chatter finding from Step 2** — a specific pain point, request, or repeated complaint you found on Reddit/X, cited back to its thread

### Step 4 — Present & iterate

Present the set grouped by theme or safe→ambitious. Ask which ideas resonate, which to cut, and whether to go deeper in a particular direction or generate a fresh batch. Iterate on request — Step 3's first pass is a starting point, not a final answer.

### Step 5 — Hand off

For any idea the user wants to pursue, point them explicitly at `solution-feasibility-study` (e.g. "run `/solution-feasibility-study` on the second one to get market/competitor/technical/monetization research and a go/no-go") rather than researching it here. Offer to save the shortlist to `project-ideas-<date>.md` in the current working directory if the user wants a persistent record — only write the file if they say yes.

## Guidelines

- **Require an anchor.** Don't generate ideas from a completely blank prompt — get at least one kick-off topic or an explicit "suggest some" first.
- **One question at a time** during the interview; keep it noticeably lighter-weight than `solution-feasibility-study`'s interview — this is meant to be fast.
- **Specific beats generic.** "An AI agent that reviews open PRs for flaky-test risk based on your team's CI history" beats "an AI code review tool." Push every pitch toward a concrete wedge.
- **Light research only.** The Step 2 X/Reddit scan plus a couple of grounding searches to keep "trending" claims honest is the ceiling here; a full competitive/market pass belongs in `solution-feasibility-study`, not here.
- **Don't fabricate trend data or threads.** If something is described as trending, newly possible, or "other founders are discussing this," it must come from an actual search performed in this session — cite the thread/post, and mark the reaction level (replies/agreement) as informal signal, not verified demand.
- **Stay in scope.** This skill ideates. It does not score feasibility, research competitors in depth, or write product code — hand off to `solution-feasibility-study` or `repo-init` for those.
