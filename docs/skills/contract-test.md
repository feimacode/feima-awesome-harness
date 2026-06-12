# contract-test — Contract Testing

**Category**: Testing &nbsp;|&nbsp; **Source**: [`skills/contract-test/SKILL.md`](../../skills/contract-test/SKILL.md)

## Purpose

Verifies the **agreement** between a consumer and a provider — not their internal behaviour. The consumer defines what it expects from the provider; the provider verifies it can meet those expectations. Contract testing is a design discipline as much as a testing technique.

**Prerequisite**: Apply [auto-test](auto-test.md) universal principles first. This skill adds contract-scope specifics.

## When to Use

- Two services are developed by different teams and must stay compatible
- A breaking API change needs to be detected before deployment
- You need to verify that a consumer only uses what the provider actually offers
- Validating that a published schema (OpenAPI, Avro, Protobuf, JSON Schema) matches actual usage

**Not for**: verifying business logic inside a service (use [unit-test](unit-test.md) or [integration-test](integration-test.md)). Not for testing that two services produce the right end-user outcome (use [e2e-test](e2e-test.md)).

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

## Smell Checklist

| Smell | Symptom | Fix |
|---|---|---|
| Overly broad contract | Contract asserts entire response body verbatim | Assert only fields the consumer reads |
| No versioning strategy | Contract files have no version; breaking changes are invisible | Tag contracts with consumer version; use a broker |
| Consumer encodes provider logic | Consumer test reproduces provider business rules | Assert on response shape, not computed values |
| No provider verification step | Consumer tests pass; provider side never runs | Wire provider verification into provider's CI pipeline |
| Shared contract file modified by both sides | Race conditions and conflicting expectations | Consumer owns the contract; provider only reads it |
| Schema drift not detected | API changes in production before contract is updated | Run provider verification on every provider deployment |

## Schema-Based Contracts

When using a schema registry (OpenAPI, Avro, JSON Schema, Protobuf):
- Consumers validate their requests and responses against the published schema.
- Providers validate their responses against the same published schema.
- Schema changes require a version bump before deployment.
- Use compatibility modes (backward, forward, full) to gate merges.

## Related Skills

- [auto-test](auto-test.md) — Universal principles (prerequisite)
- [integration-test](integration-test.md) — Real boundary testing
- [e2e-test](e2e-test.md) — Full user-facing flows
