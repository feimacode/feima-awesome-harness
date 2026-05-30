# Gemini CLI Interaction Mapping

This skill is written to be platform-neutral in `SKILL.md`. In Gemini CLI, preserve the wizard UX with these equivalents:

| Skill concept | Gemini CLI equivalent |
|---------------|-----------------------|
| Structured question/input tool | `ask_user` |
| Single-choice question | `ask_user` with `choices` |
| Free-text follow-up | `ask_user` with freeform input enabled |
| Multi-select question | Prefer native multi-select UI if available; otherwise use `ask_user` and let the user reply with a comma-separated or freeform list |
| File reads | `read_file`, `glob`, `grep_search` |
| File edits | `replace`, `write_file` |
| Shell commands | `run_shell_command` |
| Skill invocation | `activate_skill` |

## Wizard Guidance

1. Prefer `ask_user` over plain chat whenever the skill is interviewing the user.
2. Ask exactly one question per prompt.
3. Use fixed choices for enumerated answers and leave room for custom input when `Other` or `Undecided` is allowed.
4. For multi-select prompts, parse the chosen values before continuing and only confirm when needed.
