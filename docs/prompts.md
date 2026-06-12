# Prompts

Prompts are reusable templates that guide AI assistants in producing consistent, structured output for specific communication tasks.

## Available Prompts

### status-update — Incident Status Update

**Source**: [`prompts/status-update.prompt.md`](../prompts/status-update.prompt.md)

#### Purpose

Generates clear, concise status updates for stakeholders during an incident. Provides a structured template that ensures all critical information is communicated consistently.

#### Template Structure

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

### Next Steps
- [step 1]
- [step 2]

### Impact
[User-facing impact description]

### ETA
[Expected resolution time or "Under investigation"]
```

#### Usage Guidelines

- Update every 15–30 minutes during active incident
- Update when status changes (contained, resolved)
- Update when new significant findings emerge

#### Constraints

- Keep under 200 words unless critical complexity
- Always include timestamp
- Never speculate on root cause without evidence
- Never promise resolution time unless confident

#### Used By

- [incident-commander](agents.md#incident-commander) — Agent that drafts status updates during incident response
- [war-room-triage](flows.md#war-room-triage) — Flow where the commander produces stakeholder updates

#### Related Harnesses

- [incident-triage](skills/incident-triage.md) — Skill for systematic incident triage
- [incident-commander](agents.md#incident-commander) — Agent role that uses this template
