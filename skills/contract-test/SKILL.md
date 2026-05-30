---
name: contract-test
description: Use when writing or reviewing contract tests that verify the agreement between a service consumer and a service provider, including consumer-driven contracts, schema validation, and provider verification.
---

# Contract Test

## Overview

A contract test verifies the **agreement** between a consumer and a provider — not their internal behaviour. The consumer defines what it expects from the provider; the provider verifies it can meet those expectations. Contract testing is a design discipline as much as a testing technique.

**Required background:** Apply `auto-test` universal principles first. This skill adds contract-scope specifics.

## When to Use

- Two services are developed by different teams and must stay compatible
- A breaking API change needs to be detected before deployment
- You need to verify that a consumer only uses what the provider actually offers
- Validating that a published schema (OpenAPI, Avro, Protobuf, JSON Schema) matches actual usage

**Not for:** verifying business logic inside a service (use `unit-test` or `integration-test`). Not for testing that two services produce the right end-user outcome (use `e2e-test`).

## Core Principles

### Consumer Defines the Contract

The consumer writes expectations that describe only the parts of the provider response it actually uses. Not the full response shape — only the fields it reads. This prevents consumers from over-specifying and providers from breaking consumers with additive changes.

### The Contract Is a Published Artifact

A contract is not just a passing test. It is a versioned artifact (file, registry entry) shared between consumer and provider. Provider verification runs against the published contract, not the consumer's test source.

### Verify at Both Ends

| Side | Responsibility |
|---|---|
| Consumer | Write expectations; generate contract artifact; publish to broker or repo |
| Provider | Pull contract artifact; run provider verification; publish verification result |

Both sides must pass. A consumer test that passes without provider verification is incomplete.

### Contracts Are Narrow

A contract captures only the fields, status codes, and message keys the consumer actually uses. Broad contracts (assert entire response body matches exact schema) become a fragile integration test. The narrower the contract, the more safely providers can evolve.

### AAA in Contract Tests

```
# Consumer side — defines expectation
# Arrange: describe the interaction (request + expected response fragment)
# Act:     consumer code runs against the mock provider
# Assert:  consumer handles the response correctly
#          → contract artifact is generated as a side effect

# Provider side — verifies the contract
# Arrange: start the real provider
# Act:     replay each interaction from the contract artifact
# Assert:  provider response satisfies every expectation in the contract
```

## Quick Reference — Contract Smell Checklist

| Smell | Symptom | Fix |
|---|---|---|
| Overly broad contract | Contract asserts entire response body verbatim | Assert only fields the consumer reads |
| No versioning strategy | Contract files have no version; breaking changes are invisible | Tag contracts with consumer version; use a broker with version tracking |
| Consumer encodes provider logic | Consumer test reproduces provider business rules to build expected values | Assert on response shape, not computed values |
| No provider verification step | Consumer tests pass; provider side never runs | Wire provider verification into provider's CI pipeline |
| Shared contract file modified by both sides | Race conditions and conflicting expectations | Consumer owns the contract; provider only reads it |
| Schema drift not detected | API changes in production before contract is updated | Run provider verification on every provider deployment |

## Schema-Based Contracts

When using a schema registry (OpenAPI, Avro, JSON Schema, Protobuf):
- Consumers validate their requests and responses against the published schema.
- Providers validate their responses against the same published schema.
- Schema changes require a version bump before deployment.
- Use compatibility modes (backward, forward, full) to gate merges.

## Common Tools (No Prescription)

| Approach | Common tools |
|---|---|
| Consumer-driven contracts | Pact (multi-language), Spring Cloud Contract |
| Schema-based contracts | Schemathesis, Dredd, OpenAPI diff tools |
| Message/event contracts | Pact message pacts, Confluent Schema Registry |
| GraphQL contracts | GraphQL Inspector |

Principles above apply to all. For tool-specific idioms consult your team's standards.

## Common Mistakes

- **Using contract tests as integration tests** — the contract test does not verify that the provider works; it only verifies it meets the consumer's expectations.
- **Not running provider verification in CI** — a contract without provider verification gives false confidence.
- **Overspecifying the contract** — asserting on fields the consumer does not use creates unnecessary coupling between teams.
- **Treating the broker as optional** — if contracts are passed around as files manually, they go stale; use a contract broker or registry.
