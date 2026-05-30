# Status Update

A prompt template for incident status updates.

## Purpose

Generate clear, concise status updates for stakeholders during an incident.

## Template

```
## Incident Status Update

**Incident**: [brief description]
**Status**: [Investigating / Contained / Resolving / Resolved]
**Severity**: [Critical / High / Medium / Low]
**Started**: [timestamp]
**Updated**: [current timestamp]

### Current Situation
[1-2 sentences describing current state]

### Actions Taken
- [action 1]
- [action 2]
- [action 3]

### Next Steps
- [step 1]
- [step 2]

### Impact
[User-facing impact description]

### ETA
[Expected resolution time or "Under investigation"]
```

## Usage

Fill in the template based on incident commander's current picture. Update:
- Every 15-30 minutes during active incident
- When status changes (contained, resolved)
- When new significant findings emerge

## Constraints

- Keep under 200 words unless critical complexity
- Always include timestamp
- Never speculate on root cause without evidence
- Never promise resolution time unless confident