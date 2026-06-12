# e2e-test — End-to-End Testing

**Category**: Testing &nbsp;|&nbsp; **Source**: [`skills/e2e-test/SKILL.md`](../../skills/e2e-test/SKILL.md)

## Purpose

Verifies that a complete user-facing flow works correctly through the full stack. Nothing is mocked. The test interacts with the system the same way a real user or client would and asserts on observable outcomes.

**Prerequisite**: Apply [auto-test](auto-test.md) universal principles first. This skill adds e2e-scope specifics.

## When to Use

- Testing a full user journey: login → action → outcome
- Verifying that frontend, API, and database integrate correctly end to end
- Smoke testing a deployment before promoting to production
- Regression-testing critical user paths

**Not for**: logic that can be verified at the unit or integration level — e2e tests are expensive; reserve them for flows that cannot be confidently verified at a lower level.

## Core Principles

### Test Observable Behaviour Only

Assert on what a real user sees: page content, URL, HTTP status, returned JSON, terminal output. Never assert on DOM node internal IDs, CSS class names used only for styling, or internal state that users cannot observe.

### One Flow Per Test

Each test covers one user scenario from start to finish. A test that covers signup AND checkout AND refund is three tests masquerading as one. When it fails, diagnosis requires reading the whole script.

### Isolation Strategy

E2E tests leave persistent side effects (created users, seeded data, sent emails). Isolate by:

| Strategy | When to use |
|---|---|
| Per-test user account | Browser/UI flows where auth is involved |
| Data namespacing (run ID prefix) | API flows with shared data stores |
| Environment reset between runs | Full reset between CI pipeline runs |
| Read-only smoke tests | When writes cannot be safely isolated in production |

Never share a user account or data record between tests that also write to it.

### Stable Selectors

Interact with the UI through semantically meaningful selectors: accessible roles, labels, `data-testid` attributes. Never target auto-generated class names or positional selectors (`nth-child(3)`).

## Smell Checklist

| Smell | Symptom | Fix |
|---|---|---|
| Multiple flows in one test | Test covers login AND purchase AND refund | Split into three tests |
| Brittle selector | `div.css-3fx9a > span:nth-child(2)` | Use role, label, or `data-testid` |
| Sleepy test | `sleep(2000)` waiting for a page load | Use explicit condition-based wait |
| Shared auth state | All tests share one logged-in user session | Create per-test users or reset session cookie |
| No cleanup | Created records accumulate across runs | Delete or namespace test data |

## Related Skills

- [auto-test](auto-test.md) — Universal principles (prerequisite)
- [unit-test](unit-test.md) — Isolated unit logic
- [integration-test](integration-test.md) — Real boundary testing
- [performance-test](performance-test.md) — Load testing critical flows
