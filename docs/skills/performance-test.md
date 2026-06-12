# performance-test — Performance Testing

**Category**: Testing &nbsp;|&nbsp; **Source**: [`skills/performance-test/SKILL.md`](../../skills/performance-test/SKILL.md)

## Purpose

Measures non-functional properties — latency, throughput, concurrency capacity, resource consumption — under a defined load profile. Results are statistical; pass/fail is determined by comparison against an explicit SLO baseline, not exact values.

**Prerequisite**: Apply [auto-test](auto-test.md) universal principles first. This skill adds performance-scope specifics.

## When to Use

- Validating that a service meets its latency or throughput SLO before release
- Detecting regressions in response time introduced by a code change
- Sizing infrastructure for expected traffic
- Identifying bottlenecks under sustained or peak load

**Not for**: verifying correctness of logic (use [unit-test](unit-test.md) or [integration-test](integration-test.md)). Not for user-journey verification (use [e2e-test](e2e-test.md)).

## Core Principles

### SLOs Are the Assertion

Performance tests do not assert on exact values. They assert on thresholds derived from agreed SLOs:

- p95 response time ≤ 200ms
- error rate < 0.1%
- throughput ≥ 500 req/s at 100 VU

Without explicit SLO thresholds as the pass/fail gate, a performance test is a measurement exercise, not a test.

### Establish a Baseline First

The first run of a performance test establishes the baseline. Subsequent runs assert against it with a variance budget (e.g., p95 must not regress by more than 10%). Never write thresholds by guessing; measure first, then codify.

### Isolate the System Under Test

Performance results are only meaningful if the environment matches the production topology:
- Dedicated test environment; no shared traffic from other users.
- Same hardware class (or documented ratio to production).
- Warm caches and connection pools before the measurement window.
- No profilers, debug logging, or tracing overhead that would not be present in production.

### Results Are Statistical

Report percentiles (p50, p95, p99), not averages. Averages hide tail latency. A p95 of 800ms is meaningful even when the average is 100ms.

## Smell Checklist

| Smell | Symptom | Fix |
|---|---|---|
| No SLO threshold | Test runs and reports numbers but never fails | Define explicit pass/fail thresholds before running |
| No baseline | Thresholds are guessed, not measured | Run baseline first; derive thresholds from it |
| Wrong environment | Test runs on a developer laptop or shared staging | Use a dedicated, production-equivalent environment |
| Measuring averages only | Report shows mean response time only | Add p95, p99, error rate, and throughput |
| Including warmup in results | First requests skew results high | Separate ramp-up phase from measurement window |
| Testing the load tool | Network or test runner is the bottleneck | Check resource utilisation on the load generator |
| No resource monitoring | Latency is fine but CPU/memory is pegged | Collect CPU, memory, GC, and connection pool metrics |
| Single scenario | Only happy-path load is tested | Add sustained load, spike, and soak scenarios |

## Load Scenario Types

| Scenario | Purpose |
|---|---|
| Baseline load | Validate normal operating conditions |
| Peak load | Validate capacity at maximum expected traffic |
| Spike test | Validate behaviour under sudden traffic burst |
| Soak test | Validate no resource leaks under sustained load over time |

## Related Skills

- [auto-test](auto-test.md) — Universal principles (prerequisite)
- [e2e-test](e2e-test.md) — Critical user flows to load-test
- [integration-test](integration-test.md) — Boundary-level correctness
