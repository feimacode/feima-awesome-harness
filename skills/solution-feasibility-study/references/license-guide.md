# License Guide — Classifying Fork Candidates

This is a practical starting-point classification, **not legal advice**. Always tell the user to verify the actual license file and check for trademark/branding restrictions or CLA requirements before shipping anything derived from a fork commercially.

Only classify from a license actually observed (GitHub's detected-license metadata, or a fetched `LICENSE`/`LICENSE.md`/`COPYING` file). Never infer a license from a repo's name, topic tags, or README tone.

## Permissive → generally "Forkable"

Minimal obligations beyond keeping the copyright/license notice. Safe to fork, modify, and offer as a commercial product in most cases.

| License | Notes |
|---|---|
| MIT | Most permissive common license; keep the notice. |
| Apache-2.0 | Like MIT, plus an explicit patent grant; keep NOTICE file if present. |
| BSD (2-clause, 3-clause) | Similar to MIT; 3-clause adds a non-endorsement clause. |
| ISC | Functionally equivalent to MIT. |
| Unlicense / CC0 | Public-domain-equivalent; effectively no restrictions. |

## Weak Copyleft → "Caution"

Modifications to the *licensed files themselves* usually must stay under the same license and be made available, but linking/using it as a library in a separately-licensed product is typically fine. The exact boundary matters — flag it, don't resolve it for the user.

| License | Obligation to flag |
|---|---|
| MPL-2.0 | File-level copyleft: changes to MPL-licensed files must be shared under MPL; the rest of the codebase can be proprietary. |
| LGPL-2.1 / LGPL-3.0 | Must allow users to relink/replace the LGPL component; static linking has extra obligations. |

## Strong Copyleft → "Caution" (heavy obligation)

Forking and modifying is allowed, but distributing the result (including as a hosted SaaS, depending on the exact license) generally requires releasing the full source under the same terms.

| License | Obligation to flag |
|---|---|
| GPL-2.0 / GPL-3.0 | Distributing a modified version requires releasing source under GPL. |
| AGPL-3.0 | Extends GPL's obligation to network use — offering it as a hosted service also triggers source-release obligations. This is the one most likely to surprise a SaaS builder; call it out explicitly. |

## Not Forkable

- No license file at all → default copyright applies, meaning **all rights reserved**; the code is not legally reusable even though it's publicly visible on GitHub.
- Explicit "All rights reserved," proprietary, or source-available-but-no-derivatives terms (e.g., some "fair source" / BUSL variants before their change date).
- "For personal/non-commercial use only" clauses.

## Unclear → flag for the user to check directly

- Dual-licensed repos (e.g., open-source + commercial license) — the commercial terms may restrict what the free tier covers.
- A CLA (Contributor License Agreement) requirement doesn't block forking, but signals the maintainer controls the project tightly — note it.
- Explicit trademark/branding restrictions in the README or a `TRADEMARK` file (common even under MIT/Apache — the code license doesn't cover the name/logo).
- Any license text that doesn't match a standard SPDX identifier.

## Report Language

When classifying a repo in the report, always pair the classification with the specific obligation (or lack thereof), not just the license name:

> **Forkable** — MIT license, no obligations beyond retaining the copyright notice.

> **Caution** — AGPL-3.0. Forking and self-hosting is fine, but offering this as a hosted SaaS product would require releasing your modified source under AGPL-3.0 too. Confirm with counsel if you plan to keep changes proprietary.
