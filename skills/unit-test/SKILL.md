---
name: unit-test
description: Use when writing or reviewing unit tests that test a single function, class, or module in isolation with all external dependencies replaced by test doubles.
---

# Unit Test

## Overview

A unit test verifies the behaviour of one unit (function, class, or module) in complete isolation. Every dependency that crosses an architectural boundary is replaced by a test double. Speed and determinism are non-negotiable.

**Required background:** Apply `auto-test` universal principles first. This skill adds unit-scope specifics.

## When to Use

- Writing a test for a single function or class
- Reviewing unit tests for isolation or smell violations
- Deciding mocking strategy at the unit level
- Diagnosing slow, flaky, or brittle unit tests

**Not for:** tests that intentionally use real databases, real HTTP, or real filesystem — see `integration-test`.

## Core Principles

### Strict Isolation

- No real I/O: no network calls, no database, no filesystem access, no process spawns.
- No real time: no `sleep`, no wall-clock reads. Inject or fake clocks.
- One unit under test. If you find yourself setting up two real collaborators, you are writing an integration test.

### Mocking Philosophy

- Mock at the **architectural boundary**, not at every internal call.
- Mock what the unit **depends on**, not what it **is made of**.
- Prefer fakes (in-memory implementations) over mocks (interaction-asserting stubs) when the fake is simple to write.
- Never assert on how many times an internal helper was called — that tests implementation, not behaviour.

### Fixture Design

- Fixtures arrange only what the test needs. No more.
- Shared fixtures for repeated arrangement patterns; not for hiding intent.
- Avoid autouse / globally-applied fixtures unless the setup is truly universal (e.g., silence logging).

### AAA in Unit Tests

```
# GOOD — phases are clearly separated
def test_invoice_total_applies_discount_when_eligible():
    # Arrange
    items = [Item(price=100), Item(price=50)]
    customer = Customer(tier="gold")          # gold = 10% discount

    # Act
    total = calculate_invoice_total(items, customer)

    # Assert
    assert total == 135  # 150 - 10%
```

```
# BAD — arrange leaking into assert, magic constant
def test_invoice():
    assert calculate_invoice_total([Item(100), Item(50)], Customer("gold")) == 135
```

## Quick Reference — Unit Smell Checklist

| Smell | Symptom | Fix |
|---|---|---|
| Over-mocking | More mock setup lines than assertion lines | Mock only the boundary; test real logic |
| Testing internals | Assert on private method call count | Assert on output or observable state |
| Constructor logic in arrange | `new Service(new Repo(new DB(...)))` | Use factory helpers or dependency injection |
| Implicit arrange | Relies on module-level or class-level state | Make all state explicit in the test body |
| Slow unit test | Test takes > ~50ms | Find hidden I/O; inject fake clock |
| Tautological assertion | `assert result == service.compute(x)` | Assert against a known expected value |
| Multiple behaviours | One test checks happy path AND error path | Split into separate parameterised cases |

## Parameterisation Over Duplication

When the same scenario repeats with different inputs, parameterise rather than copy:

```
# Duplicated — hard to extend
def test_discount_gold(): ...
def test_discount_silver(): ...
def test_discount_none(): ...

# Parameterised — extend by adding a row
@parametrize("tier,expected", [("gold", 135), ("silver", 142.50), ("none", 150)])
def test_discount_by_tier(tier, expected): ...
```

## Common Runners (No Framework Prescription)

| Language | Common unit test runners |
|---|---|
| Python | pytest, unittest |
| JavaScript / TypeScript | Vitest, Jest |
| Java | JUnit 5, TestNG |
| .NET | xUnit, NUnit, MSTest |
| Go | built-in `testing` package |
| Rust | built-in `#[test]` |

Principles above apply to all. For runner-specific idioms, consult your team's coding standards or framework documentation.

## Common Mistakes

- **Mocking the unit under test** — if you mock the class you're testing, you're testing nothing.
- **Shared mutable state between tests** — one test mutates a module-level list; the next test gets polluted state.
- **Real time in assertions** — `assert result.created_at == datetime.now()` is always racing the clock.
- **God arrange block** — 30 lines of setup means the unit has too many dependencies; consider decomposing the unit, not adding more mocks.
