# integration-test — Integration Testing

**Category**: Testing &nbsp;|&nbsp; **Source**: [`skills/integration-test/SKILL.md`](../../skills/integration-test/SKILL.md)

## Purpose

Verifies that two or more components work correctly when wired together through a real boundary — a database, queue, HTTP client, or file system. The use of real infrastructure is intentional; that is what distinguishes it from a unit test.

**Prerequisite**: Apply [auto-test](auto-test.md) universal principles first. This skill adds integration-scope specifics.

## When to Use

- Testing that a repository layer reads/writes to a real database correctly
- Testing that an event producer puts the expected message on a queue
- Testing that two modules integrate via their real interface
- Diagnosing failures that only appear against real infrastructure

**Not for**: logic-only behaviour where a double would suffice (use [unit-test](unit-test.md)). Not for full user flows (use [e2e-test](e2e-test.md)).

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

## Smell Checklist

| Smell | Symptom | Fix |
|---|---|---|
| Too many hops | Test crosses DB + queue + HTTP in one case | Split into separate tests per boundary |
| Shared DB state | Tests pass alone but fail in suite | Add teardown or use transaction rollback |
| No cleanup strategy | Rows accumulate across runs | Wrap in transaction or delete on teardown |
| Logic buried in test | Test reproduces business rules to build expected value | Extract expected value as a constant; test only the integration |
| Slow suite | Suite takes minutes with no parallelism | Use containers; parallelise by namespace |
| Mocking the integration | Repository test mocks the database it should be testing | Use a real test database; that is the point |

## Slow vs Fast Integration Tests

Integration tests are inherently slower than unit tests. Optimise by running the real store once per suite and sharing read-only seed data across tests where possible.

## Related Skills

- [auto-test](auto-test.md) — Universal principles (prerequisite)
- [unit-test](unit-test.md) — When doubles suffice
- [e2e-test](e2e-test.md) — Full user-facing flows
- [contract-test](contract-test.md) — Consumer-provider API agreements
