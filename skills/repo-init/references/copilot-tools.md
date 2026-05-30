# Copilot CLI Interaction Mapping

This skill is written to be platform-neutral in `SKILL.md`. In Copilot CLI, preserve the wizard UX with these equivalents:

| Skill concept | Copilot CLI equivalent |
|---------------|------------------------|
| Structured question/input tool | `ask_user` |
| Single-choice question | `ask_user` with `choices` |
| Free-text follow-up | `ask_user` with `allow_freeform: true` |
| Multi-select question | Prefer native multi-select UI if available; otherwise use `ask_user` and let the user reply with a comma-separated or freeform list |
| Follow-up clarification | Another `ask_user` call, still one question at a time |
| Skill invocation | `skill` |
| File reads | `view`, `glob`, `rg` |
| File edits | `apply_patch` |

## Wizard Guidance

1. Prefer `ask_user` over plain chat whenever the skill is interviewing the user.
2. Ask exactly one question per call.
3. Use the `choices` field for fixed options rather than embedding options in prose.
4. Keep `allow_freeform: true` when the skill includes `Other`, `Undecided`, or custom answers.
5. For multi-select prompts, parse the user's answer into the selected items before moving on, and confirm only if the answer is ambiguous.
