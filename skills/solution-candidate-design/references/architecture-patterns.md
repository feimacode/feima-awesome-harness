# Architecture Patterns Cheat Sheet

A menu of standard patterns to seed **Build from Scratch** candidates. Pick the pattern whose "fits when" column matches the team size, time budget, and priority from the interview — don't default to the most familiar or most impressive-sounding pattern. Treat this as a starting sketch to adapt, not a rigid template; still ground the hardest dependency with real research per the main SKILL.md.

| Pattern | Shape | Fits when | Avoid when |
|---|---|---|---|
| **Single-server monolith** | One deployable app, one database, server-rendered or thin client | Solo/small team, weekend-to-few-months budget, MVP validation is the goal | Multiple independently-scaling components are already known requirements |
| **Modular monolith** | One deployable, internally organized into clear module boundaries (by domain, not by layer) | Small team expecting to grow, wants a migration path to services later without paying microservices tax now | Team wants strict deploy independence from day one |
| **Client + BaaS (Backend-as-a-Service)** | Frontend talks directly to a managed backend (auth, DB, storage, functions) — e.g. Supabase/Firebase/Convex/Appwrite | Solo builder, speed-to-MVP priority, standard CRUD/auth/realtime needs | Highly custom backend logic, strict data-residency/compliance needs, or heavy custom compute |
| **Serverless / event-driven** | Managed functions + managed queue/event bus + managed data stores, no servers to run | Spiky/unpredictable load, small team wanting near-zero ops burden, pay-per-use cost sensitivity | Long-running/stateful workloads, sub-100ms latency requirements, heavy local dev-loop friction is a dealbreaker |
| **Microservices** | Multiple independently deployable services, each own datastore, communicating over network (REST/gRPC/events) | Funded/multi-team org, genuinely independent scaling or ownership boundaries already exist | Solo/small team — the coordination and ops overhead usually outweighs the benefit before there's a real scaling or org-boundary need |
| **Static site + serverless functions (JAMstack)** | Statically-generated frontend, thin serverless functions for dynamic bits, third-party APIs for the rest | Content-heavy or marketing-adjacent product, small team, cost-sensitive | App has substantial real-time/stateful interaction as its core value |
| **Local-first / offline-first** | Client owns the source of truth, syncs to a backend opportunistically (CRDTs or sync engine) | Product's core value is offline reliability or local performance (note-taking, field tools) | Team has no time budget to learn sync-engine tradeoffs; strong server-side consistency requirements |
| **Native mobile + thin backend** | Native/React Native client, thin API backend, managed DB | Delivery target is mobile-first, team has or wants mobile-specific skills | Team's stated comfort is entirely web/backend with no mobile experience and no time to acquire it |

## How to use this table

1. Start from the row matching the team-size/budget/priority combination from the interview.
2. Sketch the candidate's components and data flow using that row's shape as the skeleton.
3. If the idea implies a genuinely hard technical dependency (real-time collab, ML inference, payments/PCI scope, hardware integration), layer that in explicitly and ground it with a `WebSearch` on how comparable products handle it — this table intentionally doesn't cover every specialized subsystem.
4. When two patterns are both plausible for the stated constraints, it's fine to offer both as separate candidates (e.g. "Client + BaaS" vs. "Modular monolith") rather than picking one — that's the point of this skill.
