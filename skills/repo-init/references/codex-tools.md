# Codex Interaction Mapping

This skill is written to be platform-neutral in `SKILL.md`. In Codex, use the native file and shell tools, but note that there may be no dedicated structured question tool.

| Skill concept | Codex equivalent |
|---------------|------------------|
| Structured question/input tool | No dedicated equivalent in all Codex environments — ask in chat and wait for the user's reply |
| Single-choice question | Concise numbered or bulleted options in chat |
| Free-text follow-up | Plain chat |
| Multi-select question | Ask for a comma-separated list in chat and parse the response |
| File reads/edits | Use native file tools |
| Shell commands | Use native shell tools |

## Wizard Guidance

1. Keep the interview interactive even without a dedicated question tool.
2. Ask one question at a time and wait for the reply before continuing.
3. Use short option lists that are easy to answer inline.
4. When you need multiple selections, explicitly tell the user they can answer with several items in one reply.
5. If a reply is ambiguous, restate the parsed values before moving on.
