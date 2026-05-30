---
name: e2e-test
description: Use when writing or reviewing end-to-end tests that verify observable user-facing behaviour through the full stack with no mocks, covering browser flows, API journeys, or CLI interactions against a real or production-equivalent environment.
---

# E2E Test

## Overview

An end-to-end test verifies that a complete user-facing flow works correctly through the full stack. Nothing is mocked. The test interacts with the system the same way a real user or client would and asserts on observable outcomes.

**Required background:** Apply `auto-test` universal principles first. This skill adds e2e-scope specifics.

## When to Use

- Testing a full user journey: login → action → outcome
- Verifying that frontend, API, and database integrate correctly end to end
- Smoke testing a deployment before promoting to production
- Regression-testing critical user paths

**Not for:** logic that can be verified at the unit or integration level — e2e tests are expensive; reserve them for flows that cannot be confidently verified at a lower level.

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

### AAA in E2E Tests

```
# Arrange: create test user, seed necessary data, navigate to start state
# Act:     perform the user action (click, fill, submit, API call)
# Assert:  verify the observable outcome (page text, URL, response, DB state)

test("user can complete checkout", async ({ page, testUser }) => {
  // Arrange
  await page.goto("/cart");
  await seedCart(testUser.id, [{ sku: "WIDGET-1", qty: 2 }]);

  // Act
  await page.getByRole("button", { name: "Proceed to checkout" }).click();
  await page.getByLabel("Card number").fill("4111111111111111");
  await page.getByRole("button", { name: "Pay" }).click();

  // Assert
  await expect(page.getByRole("heading", { name: "Order confirmed" })).toBeVisible();
});
```

## Quick Reference — E2E Smell Checklist

| Smell | Symptom | Fix |
|---|---|---|
| Multiple flows in one test | Test covers login AND purchase AND refund | Split into three tests |
| Brittle selector | `div.css-3fx9a > span:nth-child(2)` | Use role, label, or `data-testid` |
| Sleepy test | `sleep(2000)` waiting for a page load | Use explicit condition-based wait (wait for element/network idle) |
| Shared auth state | All tests share one logged-in user session | Create per-test users or reset session cookie |
| No cleanup | Created records accumulate across runs | Delete or namespace test data |
| Asserting internals | Checking Redux store or React component props | Assert on what the user sees |
| Re-testing lower layers | E2E test verifies discount calculation logic | That belongs in a unit test |

## Pyramid Position

E2E tests sit at the top of the testing pyramid: highest confidence, highest cost, slowest feedback. Keep the suite small and focused on critical user paths. Every scenario covered by a passing unit or integration test does not need an E2E test.

## Common Runners (No Prescription)

| Platform | Common runners |
|---|---|
| Browser (web) | Playwright, Cypress, WebdriverIO |
| Mobile | Detox, Appium, XCUITest, Espresso |
| API journey | pytest + httpx, Supertest, REST-assured |
| CLI | shell scripts + `assert` patterns, bats-core |

Principles above apply to all. For runner-specific idioms consult your team's standards.

## Common Mistakes

- **Running E2E tests in place of unit tests** — if you have 500 e2e tests and 20 unit tests, the pyramid is inverted; the suite will be slow and brittle.
- **Connecting to production** — e2e tests create, update, and delete data; always use a dedicated environment.
- **Hardcoded test credentials in source** — use secrets injection; never commit passwords or tokens.
- **Not waiting for async state** — clicking a button and immediately asserting before the response returns; use explicit waits.
