# Research Playbook

Concrete search patterns and fetch targets for each research area. Use `WebSearch` to find candidates, `WebFetch` to pull the actual page content for anything you plan to cite.

---

## 1. Market & Demand Signals

Goal: find evidence people actually have this problem and are actively looking for (or paying for) solutions today.

- `WebSearch`: `"<problem keywords>" reddit`, `"<problem keywords>" site:reddit.com`
- `WebSearch`: `"<problem keywords>" site:news.ycombinator.com`
- `WebSearch`: `"<problem keywords>" alternative to`
- `WebSearch`: `"<problem keywords>" site:producthunt.com`
- `WebSearch`: `best <category> tools 2026` (roundup posts double as a competitor seed list)

Look for: recurring complaints, "does anyone know a tool that...", people asking to be notified when something ships, paid waitlists, existing roundup/"alternatives to X" articles (their existence is itself a demand signal).

## 2. Commercial Competitor Landscape

- `WebSearch`: `<category> software` / `<category> SaaS`
- `WebSearch`: `<top competitor found> alternatives`
- `WebSearch`: `<category> site:g2.com` or `site:capterra.com` for review-site category pages
- `WebFetch` each competitor's own pricing page — do not estimate pricing from memory, pull it live.

For each competitor, capture: what they do, pricing model + price points, target segment, and one visible weakness (from reviews, a "cons" section, or gaps versus the idea).

## 3. GitHub / Open-Source Landscape (competitor research + fork candidates)

This is the differentiating research area for this skill — do it thoroughly.

### Finding candidates

- `WebSearch`: `<category keywords> site:github.com`
- `WebSearch`: `awesome <category> github` (curated "awesome-X" lists are a fast way to surface many candidates at once)
- `WebSearch`: `<category keywords> open source alternative`
- `WebSearch`: `<top commercial competitor name> open source alternative`

### Extracting data per candidate

`WebFetch` the repo's GitHub page (and README if the overview page truncates it) for each serious candidate. Record:

- **Stars** and, if visible, **stars-over-time trend** (rising/flat/declining) as a proxy for momentum
- **Last commit / last release date** — treat anything with no commits in 12+ months as low-maintenance
- **Open issues / PR backlog** — a very high ratio of open issues to stars, or PRs sitting unmerged for a long time, signals a struggling maintainer
- **Primary language(s)** — compare against the user's stated technical comfort from the interview
- **License** — read it from GitHub's detected-license badge/metadata, or fetch the actual `LICENSE`/`LICENSE.md` file if the badge is missing or ambiguous. Never infer a license from the repo name, README prose, or assumption. Classify using [license-guide.md](./license-guide.md).
- **Feature overlap** — skim the README/docs for what it actually does versus the idea being evaluated; note both the overlap and the gap.

### Classifying as a fork candidate

For every repo examined, assign one of:

- **Forkable** — permissive license (see license-guide.md), reasonably healthy (commit activity within the last ~12 months, or an old-but-stable/complete project where that's acceptable for the use case)
- **Caution** — copyleft license (MPL/LGPL/GPL/AGPL) or another license with real obligations; still usable but the report must flag exactly what obligation applies (see license-guide.md)
- **Not forkable** — no license file / "All rights reserved" / explicit no-derivatives terms
- **Unclear** — license file present but ambiguous, dual-licensed, or a CLA/trademark restriction is mentioned; flag for the user to check directly rather than guessing

Carry the **Forkable** and (with caveats) **Caution** entries into the report's Fork Candidates section per the main SKILL.md.

## 4. Technical Feasibility

- Identify the hardest technical dependency the idea implies (e.g., real-time collaboration, payments/PCI scope, ML/inference infra, hardware integration, marketplace/app-store approval, data pipeline at scale).
- `WebSearch` for how comparable products are typically built (`"<hard dependency>" how to build`, `"<hard dependency>" architecture`) to sanity-check effort.
- Compare required stack against the user's stated technical comfort from the interview; flag any gap explicitly rather than assuming they'll pick it up.
- Bucket the estimate: **Weekend prototype**, **A few months part-time**, **6-12 months**, **Needs a team/funding** — matched against their stated time budget.

## 5. Monetization Comparables

- Use the pricing pulled in step 2 to sanity-check a plausible price point and model for this idea.
- `WebSearch`: `<category> pricing benchmark` or `<category> average price`.
- For a solo/small-team builder, flag whether the model requires enterprise sales motion (long cycles, a team the user doesn't have) versus self-serve/PLG (matches solo capacity better).

## 6. Legal, Regulatory & Trademark Flags

- Check whether the domain implies regulated data (health → HIPAA-adjacent, finance/payments → PCI/money-transmission, EU users → GDPR, children's data → COPPA) and name the specific regime, not just "there might be compliance stuff."
- `WebSearch`: `<proposed product name> trademark` and `<proposed product name>` generally, to check for name collisions with the competitors already found.
- This is a flag-raising pass, not a legal opinion — say so in the report.

## 7. Go-to-Market Channels

- Base recommendations on the team-size/time-budget answers from the interview. A solo weekend builder gets community/content/SEO/PLG suggestions, not "hire an SDR team."
- Look at where the competitors found in step 2 (both commercial and OSS) actually show up — their own marketing pages, changelogs, and community links (Discord/Slack, GitHub Discussions) are a realistic channel map for this specific niche.
