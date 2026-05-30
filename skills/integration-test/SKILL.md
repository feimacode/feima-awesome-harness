---
name: integration-test
description: Use when writing or reviewing tests that verify a real integration boundary such as database queries, message queue interactions, external service calls, or inter-module wiring where real infrastructure is intentional.
---

# Integration Test

## Overview

An integration test verifies that two or more components work correctly when wired together through a real boundary — a database, queue, HTTP client, or file system. The use of real infrastructure is intentional; that is what distinguishes it from a unit test.

**Required background:** Apply `auto-test` universal principles first. This skill adds integration-scope specifics.

## When to Use

- Testing that a repository layer reads/writes to a real database correctly
- Testing that an event producer puts the expected message on a queue
- Testing that two modules integrate via their real interface
- Diagnosing failures that only appear against real infrastructure

**Not for:** logic-only behaviour where a double would suffice (use `unit-test`). Not for full user flows (use `e2e-test`).

## Core Principles

### One Integration Boundary Per Test

Each test targets **one integration point**. A test that seeds a database, calls an HTTP endpoint, reads a queue, and asserts on a file is an e2e test in disguise — it becomes expensive to diagnose and brittle when any component changes.

### Isolation Strategy

Real infrastructure introduces shared state. Each test must leave the world in the same state it found it.

| Strategy | When to use |
|---|---|
| Database transaction rollback | Same-process tests with a single DB connection |
| Container per test run | Tests that need a clean schema or different DB engines |
| Data namespacing (tenant/prefix) | When rollback is not available; isolate by key prefix or test-run ID |
| Seed + teardown scripts | When declarative reset is simpler than transaction management |

Never rely on test execution order to set up state for the next test.

### Scope Boundary

The integration test asserts on **the boundary output**: the row in the database, the message on the queue, the HTTP response status and body. It does not re-test business logic that is already covered by unit tests.

### AAA in Integration Tests

```
# Arrange: seed the database with known state
# Act: call the component under test (e.g. repository method, HTTP handler)
# Assert: query the real store for the expected outcome

def test_order_repository_persists_new_order(db_session):
    # Arrange
    customer = CustomerFactory.create(session=db_session)

    # Act
    repo = OrderRepository(db_session)
    order = repo.create(customer_id=customer.id, total=99.00)

    # Assert
    row = db_session.query(Order).get(order.id)
    assert row.total == Decimal("99.00")
    assert row.customer_id == customer.id
```

## Quick Reference — Integration Smell Checklist

| Smell | Symptom | Fix |
|---|---|---|
| Too many hops | Test crosses DB + queue + HTTP in one case | Split into separate tests per boundary |
| Shared DB state | Tests pass alone but fail in suite | Add teardown or use transaction rollback |
| No cleanup strategy | Rows accumulate across runs | Wrap in transaction or delete on teardown |
| Logic buried in test | Test reproduces business rules to build expected value | Extract expected value as a constant; test only the integration |
| Slow suite | Suite takes minutes with no parallelism | Use containers; parallelise by namespace |
| Mocking the integration | Repository test mocks the database it should be testing | Use a real test database; that is the point |

## Slow vs Fast Integration Tests

Integration tests are inherently slower than unit tests. Optimise by:
- Running the real store once per suite (container lifecycle), not once per test.
- Sharing read-only seed data across tests; isolating only write paths.
- Parallelising tests that use distinct namespaces.

Do not sacrifice isolation for speed. A fast test that produces false positives is worse than a slow reliable one.

## Common Runners and Infrastructure Tools (No Prescription)

| Need | Common tools |
|---|---|
| Container lifecycle | Testcontainers (Java, Go, Python, .NET, Node) |
| In-memory DB | H2 (Java), SQLite, DynamoDB local |
| Real DB per run | Docker Compose, GitHub Actions services |
| HTTP stubbing for third-party | WireMock, Nock, responses (Python) |

Principles above apply to all. For runner-specific idioms consult your team's standards.

## Common Mistakes

- **Unit-testing business logic inside integration tests** — integration tests should be thin; fat test bodies indicate missing unit coverage.
- **Skipping teardown because "it will be reset anyway"** — other tests in the same run will see the pollution.
- **Connecting to a shared staging DB** — integration tests need their own isolated infrastructure.
- **Treating integration test failures as "environment flakiness"** — if the test fails against a real DB, investigate the failure; do not skip or retry blindly.
