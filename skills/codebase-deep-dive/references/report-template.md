# Codebase Deep Dive — `<repo-slug>`

**Source**: `<GitHub URL or local path>` &nbsp;|&nbsp; **Access**: `<Public GitHub / Private GitHub / Local path / Pasted-only>` &nbsp;|&nbsp; **Depth**: `<Quick orientation / Deep dive>` &nbsp;|&nbsp; **Date**: `<date>`

## 1. Idea & Positioning

- **Pitch**: `<one line — stated or "inferred, not stated">`
- **Target user / use case**: `<...>`
- **Positioning vs. alternatives**: `<if stated in README/docs, else "not stated">`

## 2. Architecture Overview

- **Closest pattern**: `<from architecture-patterns.md, or "hybrid/does not cleanly fit">`
- **Component/data-flow sketch**:
  ```
  <short bullets or text diagram>
  ```
- **Stack**: `<languages / frameworks / datastore(s) / deployment model — each cited to the file it was confirmed in>`

## 3. Implementation Details *(Deep dive only)*

| Area | Notes |
|---|---|
| Entry point(s) | |
| Core modules | `<module> — <responsibility>`, one row per module |
| Notable patterns/logic worth learning from | |
| Key dependencies & why | `<dependency> — <what it's doing here>` |
| Testing & CI | |
| Extension points (best places to fork/hook in) | |

## 4. Health & Risk Signals

- **License**: `<classification per license-guide.md, with the specific obligation stated — not legal advice>`
- **Maintenance activity** *(external repos)*: last commit `<date>`, release cadence `<...>`, contributors `<n>`, open issues/PRs `<n>` — or "not applicable (internal repo)"
- **Tech debt spot-check**: `<outdated deps, thin test coverage, TODO density — explicitly a spot-check, not exhaustive>`
- **Ownership/bus-factor** *(internal repos, if in scope)*: `<structural note only>`
- **Secrets found**: `<"none observed" / "found in <file> — value not reproduced here, rotate/remove before any further use">`

## 5. Bottom Line

2-3 sentences: what this repo is, how solid a foundation it is for the stated purpose (fork candidate / learning reference / onboarding), and the single biggest thing to verify before relying on it further.

## 6. Sources

Every URL fetched or file path read this session, so findings can be re-verified.

---

*This is a point-in-time extraction from what was actually fetched/read during this session — not a security audit or legal review. Anything not directly verifiable is marked "unverified" or "not available" above rather than guessed.*
