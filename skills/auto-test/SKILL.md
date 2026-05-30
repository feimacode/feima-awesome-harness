---
name: auto-test
description: Use when writing, reviewing, or diagnosing any automated test regardless of scope or framework. Triggers on: writing new tests, reviewing test quality, identifying test smells, naming tests, structuring test suites, or deciding which scope skill to apply.
---

# Auto-Test: Universal Testing Principles

## Overview

Core principles that apply to every automated test regardless of scope (unit, integration, e2e, contract, performance) or framework. Scope skills extend these; they do not replace them.

## When to Use

- Writing any automated test from scratch
- Reviewing tests for quality or smells
- Diagnosing flaky, brittle, or unreadable tests
- Deciding which scope skill to invoke next

After applying universal principles, identify the test scope and load the corresponding skill:

| Scope | Load skill |
|---|---|
| Testing one function/class in isolation | `unit-test` |
| Testing a real integration boundary (DB, queue, service) | `integration-test` |
| Testing full user-facing flows through the live stack | `e2e-test` |
| Testing the contract between a consumer and provider | `contract-test` |
| Testing latency, throughput, or resource behaviour under load | `performance-test` |

## Universal Principles

### Structure & Readability

- **AAA (Arrange-Act-Assert)** — one clear setup block, one action, one verification block. Never interleave them.
- **Single behaviour per test** — one logical concern per test, not necessarily one `assert` call.
- **Descriptive naming** — name encodes scenario and expected outcome. Pattern: `should_<outcome>_when_<condition>` or `<unit>_<scenario>_<expectation>`.
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
- A test must be readable without tracing into fixtures — the reader should understand what is tested from the test body alone.
- Delete tests that duplicate coverage without adding clarity.

## How Principles Adapt by Scope

| Principle | Unit | Integration | E2E | Contract | Performance |
|---|---|---|---|---|---|
| External dependencies | Always double | Real by design | Real by design | Stubbed provider | Real target system |
| Isolation strategy | Pure in-process | Transaction rollback, container reset | Per-test accounts, data namespacing | Published artifact | Environment isolation, baseline variance budget |
| AAA mapping | Setup → call → assert | Seed → trigger → assert DB/API state | Navigate/seed → user action → visible outcome | Consumer expectation → provider verification | Configure load → run → assert vs SLO |
| Determinism concern | Logic-level | Data state | UI state + network | Schema version | Statistical; requires baseline + variance budget |

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
| Setup leakage | Fixture or before-hook does real work that belongs in arrange |
| No assertion | Test body has no assertion; passes vacuously |

## Common Mistakes

- **Skipping AAA for "simple" tests** — even a two-line test benefits from a clear act boundary.
- **Naming tests after methods** — `test_save()` says nothing about the scenario or expected outcome.
- **One test file per class, not per behaviour** — leads to bloated files where unrelated scenarios share fixtures.
- **Asserting on internals** — verifying which private methods were called rather than what the unit produces.
