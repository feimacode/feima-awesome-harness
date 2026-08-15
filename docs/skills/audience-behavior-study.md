# audience-behavior-study — Audience & User Behavior Study

**Category**: Audience Research &nbsp;|&nbsp; **Source**: [`skills/audience-behavior-study/SKILL.md`](../../skills/audience-behavior-study/SKILL.md)

## Purpose

Takes an idea's target user from a one-line description to grounded, evidence-backed audience understanding — who they are, what they complain about today, what language they use, what they've already tried, what would make them pay, and where they actually hang out online. Mines real public conversations (Reddit, X/Twitter, review sites, niche communities) instead of inventing personas from imagination. It's the natural next step once [solution-feasibility-study](solution-feasibility-study.md) gives a Go/Conditional Go call: feasibility asks "should we build this," this skill asks "who exactly are we building for, and how do we talk to them."

## When to Use

- After a `solution-feasibility-study` Go/Conditional Go call, before building or writing any copy
- Wanting real pain-point quotes and the exact language a target audience uses, instead of guessed personas
- Mapping which communities (subreddits, X, Discord/Slack, forums) an audience actually lives in
- Mining competitor review sites for recurring feature-gap complaints

## Workflow

1. **Intake** — reads a `feasibility-study-*.md` file in the working directory if one exists, to reuse the idea/ICP/market-signal context; otherwise asks for the idea and target user directly.
2. **Scoping interview** — one question at a time: target segment(s), which communities/platforms to prioritize, research goal (positioning & messaging vs. persona/JTBD depth vs. channel mapping vs. all), and research depth (quick scan vs. deep dive).
3. **Research** — executes the [research playbook](../../skills/audience-behavior-study/references/research-playbook.md) via web search and page fetches:
   - Pain-point mining with verbatim quotes
   - Current workarounds people already use
   - Objections & skepticism, grounded in real complaints
   - Willingness-to-pay signals
   - A vocabulary/language glossary — the words users actually use, versus internal jargon
   - Review-site mining (G2/Capterra/App Store) for named competitors
   - Community/channel map with activity signals
4. **Synthesis** — clusters findings into 2-4 personas (jobs-to-be-done, pain points with quotes, workarounds, objections, willingness-to-pay, language glossary), plus a cross-persona theme summary and a channel map matched to realistic team capacity.
5. **Report** — fills the [report template](../../skills/audience-behavior-study/references/report-template.md), writes `audience-study-<idea-slug>.md` to the working directory, and presents a condensed summary in chat.
6. **Hand off** — points to using the glossary/quotes for landing-page copy, feeding the channel map back into a feasibility report's go-to-market section, or running real user interviews once actual users exist.

## Evidence Discipline & Boundaries

Every quote, thread, or signal must trace to something actually fetched during the session — cited with a link, or marked "unverified." This is public, aggregate listening research only: no personal data collection beyond what's already publicly visible, no suggestions to contact or target individuals, and no scraping techniques that bypass a platform's ToS or rate limits. It is explicitly framed as an informal-signal starting hypothesis, not a substitute for direct user interviews.

## References

- [Research playbook](../../skills/audience-behavior-study/references/research-playbook.md) — concrete search/fetch patterns per research area
- [Report template](../../skills/audience-behavior-study/references/report-template.md) — the structure filled in for the final report

## Related Skills

- [solution-feasibility-study](solution-feasibility-study.md) — run this first for the Go/Conditional Go call this skill builds on
- [project-idea-brainstorm](project-idea-brainstorm.md) — find an idea to feed into the pipeline if you don't have one yet
- [solution-candidate-design](solution-candidate-design.md) — turn the idea into concrete, comparable build options, grounded partly in the pain points found here
- [codebase-deep-dive](codebase-deep-dive.md) — go deep on a specific fork candidate's architecture and implementation
- [repo-init](repo-init.md) — scaffold a new repository once a build direction is chosen
- [ai-config](ai-config.md) — set up AI assistant configs after choosing to build
