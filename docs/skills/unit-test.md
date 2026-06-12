# unit-test — Unit Testing

**Category**: Testing &nbsp;|&nbsp; **Source**: [`skills/unit-test/SKILL.md`](../../skills/unit-test/SKILL.md)

## Purpose

Verifies the behaviour of one unit (function, class, or module) in complete isolation. Every dependency that crosses an architectural boundary is replaced by a test double. Speed and determinism are non-negotiable.

**Prerequisite**: Apply [auto-test](auto-test.md) universal principles first. This skill adds unit-scope specifics.

## When to Use

- Writing a test for a single function or class
- Reviewing unit tests for isolation or smell violations
- Deciding mocking strategy at the unit level
- Diagnosing slow, flaky, or brittle unit tests

**Not for**: tests that intentionally use real databases, real HTTP, or real filesystem — see [integration-test](integration-test.md).

## Core Principles

### Strict Isolation

- No real I/O: no network calls, no database, no filesystem access, no process spawns.
- No real time: no `sleep`, no wall-clock reads. Inject or fake clocks.
- One unit under test. Setting up two real collaborators means you're writing an integration test.

### Mocking Philosophy

- Mock at the **architectural boundary**, not at every internal call.
- Mock what the unit **depends on**, not what it **is made of**.
- Prefer fakes (in-memory implementations) over mocks (interaction-asserting stubs) when the fake is simple.
- Never assert on how many times an internal helper was called — that tests implementation, not behaviour.

### Fixture Design

- Fixtures arrange only what the test needs. No more.
- Shared fixtures for repeated arrangement patterns; not for hiding intent.
- Avoid autouse / globally-applied fixtures unless the setup is truly universal.

## Smell Checklist

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

When the same scenario repeats with different inputs, parameterise rather than copy. This keeps the test surface readable and makes adding new cases trivial.

## Related Skills

- [auto-test](auto-test.md) — Universal principles (prerequisite)
- [integration-test](integration-test.md) — When real boundaries are needed
- [e2e-test](e2e-test.md) — Full user-facing flows
