# Extraction Checklist

Concrete lookup patterns per source-material category. Use the column matching the access type from Step 1. Every cell should produce something actually fetched/read — if a lookup comes back empty, mark that item "not available" rather than skipping silently.

| What to find | Public/private GitHub repo | Local filesystem path |
|---|---|---|
| Idea/pitch | `WebFetch` the repo's README (raw or rendered); check for a linked landing/docs site and `WebFetch` that too | `Read` `README.md` at repo root; `Glob` for `docs/index.*` or a linked site in the README |
| Directory/module layout | `gh api repos/:owner/:repo/contents` (or `WebFetch` the repo's GitHub file-tree page) for the top 1-2 levels | `Glob` `**/*` capped to top 1-2 levels, or run `ls`/`tree` style listing via Bash if available |
| Stack/dependencies | `WebFetch` the manifest file(s): `package.json`, `pyproject.toml`, `requirements.txt`, `go.mod`, `Cargo.toml`, `pom.xml`, `*.csproj` — whichever exist | `Read` the same manifest file(s) directly |
| Entry points | `WebFetch`/`gh api` the paths referenced by the manifest's `main`/`scripts`/`bin` fields, or conventional locations (`cmd/`, `src/index.*`, `main.*`) | `Glob` for the same conventional locations; `Read` the top of each candidate file |
| Architecture/design docs | `WebFetch` `/docs`, `/adr`, `/architecture.md`, or similar if linked from the README | `Glob` for the same paths; `Read` any found |
| Testing & CI | `WebFetch` `.github/workflows/*.yml` or equivalent CI config; check for a `tests/`/`spec/` directory in the file tree | `Read` CI config files directly; `Glob` for test directories |
| License | `gh api repos/:owner/:repo` (detected `license` field) or `WebFetch` the `LICENSE`/`LICENSE.md`/`COPYING` file directly — never infer from repo name/description | `Read` `LICENSE`/`LICENSE.md`/`COPYING` at repo root |
| Maintenance activity | `gh api repos/:owner/:repo` for pushed_at, `gh api repos/:owner/:repo/contributors`, `gh api repos/:owner/:repo/releases` for cadence, open issues/PR counts | Not applicable — use `git log -1`, `git shortlog -sn` if it's a git working copy, otherwise skip and mark "not applicable (internal, no external activity signal)" |
| Ownership signals (internal only) | N/A | `Read` `CODEOWNERS` if present; `git log --format='%an' -- <path> | sort | uniq -c` for concentration, kept structural — no individual commentary |
| Secrets check | Skim fetched config/env-example files for `KEY=`, `SECRET=`, `TOKEN=` patterns — flag location only, never reproduce the value | Same, via `Grep` for common secret-pattern keywords across config files |

## Time-boxing

- **Quick orientation**: README + top-level directory listing + manifest file only. Skip Step 5 (Implementation Deep-Dive) and Step 6's tech-debt/ownership sub-items entirely.
- **Deep dive**: everything in the table above, plus 1-2 `Read`/`WebFetch` passes into the 2-3 most important core modules identified from the directory layout.

## What "actually fetched" means here

A claim like "uses PostgreSQL via Prisma" is only assertable after actually reading `package.json`'s dependencies (or the equivalent) — not because the README says "production-ready" or the repo has a database-shaped name. If a manifest or config file can't be reached, say so explicitly rather than inferring the stack from adjacent signals (stars, topics, README prose).
