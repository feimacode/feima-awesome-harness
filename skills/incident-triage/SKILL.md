# Incident Triage

A skill for systematically triaging production incidents.

## Purpose

When an incident occurs, this skill guides the AI to:
1. Gather initial context (error logs, recent changes, affected systems)
2. Identify the likely scope and severity
3. Propose immediate containment actions
4. Recommend investigation paths

## When to Use

- Production error or outage detected
- User reports system unavailable or degraded
- Monitoring alerts trigger
- Need structured approach to incident response

## Approach

### Step 1: Gather Context

Ask for:
- Error messages or stack traces
- Timeline of when issue started
- Recent deployments or changes
- Affected components/services

### Step 2: Assess Severity

Classify:
- **Critical**: User-facing outage, data loss risk
- **High**: Significant degradation, limited user impact
- **Medium**: Feature unavailable, workaround exists
- **Low**: Minor issue, cosmetic or edge case

### Step 3: Containment

Propose:
- Immediate mitigations (rollback, disable feature, scale resources)
- Communication needs (stakeholders, users)
- War room or coordination setup

### Step 4: Investigation

Suggest:
- Logs to examine
- Metrics to check
- Code paths to review
- Hypotheses to test

## Output Format

```
## Incident Summary
- **Severity**: [Critical/High/Medium/Low]
- **Scope**: [components affected]
- **Timeline**: [when started, when detected]

## Immediate Actions
1. [containment step]
2. [containment step]

## Investigation Paths
- [path 1 with rationale]
- [path 2 with rationale]

## Recommended Next Steps
- [step 1]
- [step 2]
```

## Constraints

- Do not propose fixes without understanding root cause
- Do not suggest changes to production without explicit approval
- Always confirm severity classification with human
- Prioritize containment over root cause in critical incidents