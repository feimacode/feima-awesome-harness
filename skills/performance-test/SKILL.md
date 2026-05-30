---
name: performance-test
description: Use when writing or reviewing performance tests that measure latency, throughput, resource utilisation, or scalability under load, and when asserting results against SLO thresholds as a pass/fail gate.
---

# Performance Test

## Overview

A performance test measures non-functional properties — latency, throughput, concurrency capacity, resource consumption — under a defined load profile. Results are statistical; pass/fail is determined by comparison against an explicit SLO baseline, not exact values.

**Required background:** Apply `auto-test` universal principles first. This skill adds performance-scope specifics.

## When to Use

- Validating that a service meets its latency or throughput SLO before release
- Detecting regressions in response time introduced by a code change
- Sizing infrastructure for expected traffic
- Identifying bottlenecks under sustained or peak load

**Not for:** verifying correctness of logic (use `unit-test` or `integration-test`). Not for user-journey verification (use `e2e-test`).

## Core Principles

### SLOs Are the Assertion

Performance tests do not assert on exact values. They assert on thresholds derived from agreed SLOs:

- p95 response time ≤ 200ms
- error rate < 0.1%
- throughput ≥ 500 req/s at 100 VU

Without explicit SLO thresholds as the pass/fail gate, a performance test is a measurement exercise, not a test.

### Establish a Baseline First

The first run of a performance test establishes the baseline. Subsequent runs assert against it with a variance budget (e.g., p95 must not regress by more than 10%). Never write thresholds by guessing; measure first, then codify.

### AAA in Performance Tests

```
# Arrange: configure the load profile (virtual users, ramp-up, duration)
#          seed any necessary data; isolate the system under test
# Act:     run the load scenario
# Assert:  evaluate results against SLO thresholds; fail the test if any threshold is breached
```

### Isolate the System Under Test

Performance results are only meaningful if the environment matches the production topology:
- Dedicated test environment; no shared traffic from other users.
- Same hardware class (or documented ratio to production).
- Warm caches and connection pools before the measurement window.
- No profilers, debug logging, or tracing overhead that would not be present in production.

### Results Are Statistical

Report percentiles (p50, p95, p99), not averages. Averages hide tail latency. A p95 of 800ms is meaningful even when the average is 100ms.

## Quick Reference — Performance Smell Checklist

| Smell | Symptom | Fix |
|---|---|---|
| No SLO threshold | Test runs and reports numbers but never fails | Define explicit pass/fail thresholds before running |
| No baseline | Thresholds are guessed, not measured | Run baseline first; derive thresholds from it |
| Wrong environment | Test runs on a developer laptop or shared staging | Use a dedicated, production-equivalent environment |
| Measuring averages only | Report shows mean response time only | Add p95, p99, error rate, and throughput |
| Including warmup in results | First requests skew results high | Separate ramp-up phase from measurement window |
| Testing the load tool | Network or test runner is the bottleneck, not the service | Check resource utilisation on the load generator |
| No resource monitoring | Latency is fine but CPU/memory is pegged | Collect CPU, memory, GC, and connection pool metrics alongside response times |
| Single scenario | Only happy-path load is tested | Add sustained load, spike, and soak scenarios for completeness |

## Load Scenario Types

| Scenario | Purpose |
|---|---|
| Baseline load | Validate normal operating conditions |
| Peak load | Validate capacity at maximum expected traffic |
| Spike test | Validate behaviour under sudden traffic burst |
| Soak test | Validate no resource leaks under sustained load over time |
| Stress test | Find the breaking point; not a pass/fail gate |

Start with baseline and peak. Add soak only when memory leaks or connection exhaustion are a concern.

## Common Tools (No Prescription)

| Language / style | Common tools |
|---|---|
| Script-based | k6 (JS), Locust (Python), Gatling (Scala/Java) |
| GUI / recorded | Apache JMeter |
| HTTP benchmarking | wrk, hey, autocannon |
| Continuous profiling | Pyroscope, Clinic.js, async-profiler |

Principles above apply to all. For tool-specific idioms consult your team's standards.

## Common Mistakes

- **Not isolating the system under test** — a busy CI runner shares CPU with the load generator and the service; results are meaningless.
- **Testing once and declaring victory** — performance results vary by run; collect multiple runs and report statistical confidence intervals.
- **Asserting on absolute response times without environment parity** — a 50ms result on a developer machine and a 250ms result in production are not comparable.
- **Skipping soak tests before launch** — memory leaks and connection pool exhaustion only surface after hours of sustained load, not in a 2-minute baseline run.
