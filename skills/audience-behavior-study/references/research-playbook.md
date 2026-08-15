# Research Playbook

Concrete search patterns and fetch targets for each research area. Use `WebSearch` to find candidate threads/pages, `WebFetch` to pull the actual content for anything you plan to quote or cite. Stay at normal search/fetch volume — this is a listening pass, not a scrape.

---

## 1. Pain-Point Mining

Goal: find people describing the problem in their own words, unprompted.

- `WebSearch`: `site:reddit.com "<problem keywords>" frustrating OR annoying OR "wish there was"`
- `WebSearch`: `site:reddit.com <relevant subreddit> "<problem keywords>"`
- `WebSearch`: `site:x.com OR site:twitter.com "<problem keywords>" "anyone else" OR "so annoying" OR "why is there no"`
- `WebSearch`: `"<problem keywords>" site:news.ycombinator.com`
- `WebSearch`: `"<problem keywords>" "does anyone know a tool" OR "looking for a tool"`

For each promising thread, `WebFetch` it if the search snippet is too thin to pull a real quote. Record the verbatim sentence (not a paraphrase), the link, the platform, and a rough date.

## 2. Current Workarounds

- `WebSearch`: `"<problem keywords>" "I use" OR "I currently" OR "workaround" OR "instead I"`
- `WebSearch`: `"<problem keywords>" spreadsheet OR "manually" OR "cobbled together"`

Look for what people are duct-taping together today (spreadsheets, generic tools misused for this purpose, manual processes) — this is often the real competitor, not another startup.

## 3. Objections & Skepticism

- `WebSearch`: `"<category or competitor name>" "didn't work" OR "gave up on" OR "switched away from"`
- `WebSearch`: `"<category>" reddit "waste of money" OR "overpriced" OR "too complicated"`
- Review-site "cons" sections (see §5) are a rich source of concrete objections tied to a real product.

Capture the *specific* reason for skepticism, not a generic "some people didn't like it."

## 4. Willingness-to-Pay Signals

- `WebSearch`: `"<problem keywords>" "I'd pay for" OR "shut up and take my money" OR "would pay"`
- `WebSearch`: `"<category>" pricing reddit complaints` (people complaining about price still implies willingness to pay *something*, and often states an anchor number)
- Note any explicit dollar figures mentioned, waitlists, or paid-community gatekeeping (e.g. a paywalled Discord/newsletter for this exact niche is a strong signal).

Classify each signal: **strong** (explicit "I'd pay $X"), **weak** (interest without a price), or **none found**.

## 5. Vocabulary / Language Glossary

While reading every thread above, keep a running list of the exact phrases real users use for the problem, the solution category, and their own role/context — as opposed to how the idea's team might describe it internally. This glossary is often the single most useful output of this skill for later landing-page/ad copy.

- Prefer their words over your synonyms, even if the phrasing sounds informal or is domain slang.
- Note phrases that recur across multiple independent threads — those are the safest to reuse in copy.

## 6. Review-Site Mining (when competitor products already exist)

If a prior `solution-feasibility-study` (or this session's own research) surfaced named competitor products:

- `WebFetch` the competitor's G2/Capterra/TrustRadius review page, or App Store/Play Store listing.
- Sort or scan for the lowest-rated reviews and any "cons" sections.
- Extract: the specific feature gap or friction point named, and how often the same gap recurs across reviews.

Feature gaps repeated across multiple reviews of the same product are strong differentiation material.

## 7. Community / Channel Map

- `WebSearch`: `best subreddits for <audience/topic>`
- `WebSearch`: `<topic> discord community` / `<topic> slack community`
- `WebSearch`: `<topic> newsletter` / `<topic> "build in public"` (creator-adjacent audiences often cluster around specific newsletters or X lists)
- For each community found, note: platform, rough size (subscriber/member count if visible), activity level (are there recent posts, or is it dormant), and what kind of content gets engagement there (based on the threads already pulled in §1-4).

Match the channel recommendations to a realistic capacity — a solo/small-team builder should get organic/community-participation channels first, not a paid-acquisition strategy, unless the feasibility study indicated a funded team.
