# incident-triage — Incident Triage

**Category**: Incident Response &nbsp;|&nbsp; **Source**: [`skills/incident-triage/SKILL.md`](../../skills/incident-triage/SKILL.md)

## Purpose

Systematically triage production incidents — gather context, assess severity, propose containment, and recommend investigation paths.

## When to Use

- Production error or outage detected
- User reports system unavailable or degraded
- Monitoring alerts trigger
- Need a structured approach to incident response

## Workflow

### Step 1: Gather Context

Collect:
- Error messages or stack traces
- Timeline of when the issue started
- Recent deployments or changes
- Affected components/services

### Step 2: Assess Severity

| Level | Criteria |
|---|---|
| **Critical** | User-facing outage, data loss risk |
| **High** | Significant degradation, limited user impact |
| **Medium** | Feature unavailable, workaround exists |
| **Low** | Minor issue, cosmetic or edge case |

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
- Always confirm severity classification with a human
- Prioritize containment over root cause in critical incidents

## Related Harnesses

- [incident-commander](../agents.md) — Agent role for coordinating multi-specialist incident response
- [war-room-triage](../flows.md) — Multi-role flow orchestrating incident investigation across layers
- [status-update](../prompts.md) — Prompt template for stakeholder status updates
