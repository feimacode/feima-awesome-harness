# auto-test — Universal Testing Principles

**Category**: Testing &nbsp;|&nbsp; **Source**: [`skills/auto-test/SKILL.md`](../../skills/auto-test/SKILL.md)

## Purpose

Core principles that apply to every automated test regardless of scope (unit, integration, e2e, contract, performance) or framework. This is the entry point for all testing work — scope-specific skills extend these principles, they do not replace them.

## When to Use

- Writing any automated test from scratch
- Reviewing tests for quality or smells
- Diagnosing flaky, brittle, or unreadable tests
- Deciding which scope-specific skill to invoke next

## Core Principles

### Structure & Readability

- **AAA (Arrange-Act-Assert)** — one clear setup block, one action, one verification block. Never interleave them.
- **Single behaviour per test** — one logical concern per test.
- **Descriptive naming** — name encodes scenario and expected outcome. Pattern: `should_<outcome>_when_<condition>`.
- **No logic in tests** — no `if`, `for`, `switch` inside test bodies. Use parameterisation instead.

### Isolation

- Tests must not share mutable state — no shared variables mutated across tests.
- No dependency on execution order — each test is self-sufficient.
- Test doubles are minimal — mock only at architectural boundaries, not implementation details.

### Reliability

- **Deterministic** — same inputs always produce same result.
- **No time-coupling** — no `sleep`, no wall-clock assertions, no fixed timestamps.
- **No ambient global state** — tests clean up after themselves.

### Scope

- Test the contract, not the implementation — verify outputs and observable side effects, not internal call sequences.
- Boundary and edge cases are explicit — null, empty, overflow, error paths are tested deliberately.

### Maintainability

- DRY only where it aids clarity — shared setup for repeated arrangement, not for hiding test intent.
- A test must be readable without tracing into fixtures.
- Delete tests that duplicate coverage without adding clarity.

## Universal Smell Taxonomy

| Smell | Description |
|---|---|
| Interleaved AAA | Arrange and assert mixed into the act block |
| Mystery guest | Test depends on state set up outside the test body |
| Eager test | Multiple behaviours asserted in one test |
| Sensitive equality | Asserting on exact timestamps, IDs, or generated values |
| Commented-out test | Dead test neither deleted nor explained |
| Tautological assertion | Assert that `x == x` or mirrors the implementation trivially |
| Sleepy test | `sleep` or fixed timeout instead of condition-based waiting |

## Scope Selection Guide

After applying universal principles, identify the test scope:

| Scope | Load skill | When |
|---|---|---|
| One function/class in isolation | [unit-test](unit-test.md) | All dependencies mocked |
| Real integration boundary | [integration-test](integration-test.md) | DB, queue, HTTP, filesystem |
| Full user-facing flow | [e2e-test](e2e-test.md) | Nothing mocked, real stack |
| Consumer-provider agreement | [contract-test](contract-test.md) | API compatibility between services |
| Latency/throughput under load | [performance-test](performance-test.md) | SLO validation, capacity planning |

## Related Skills

- [unit-test](unit-test.md) — Unit-level isolation and mocking
- [integration-test](integration-test.md) — Real boundary testing
- [e2e-test](e2e-test.md) — Full-stack user flows
- [contract-test](contract-test.md) — Consumer-provider contracts
- [performance-test](performance-test.md) — Load and latency testing
