# Context Management Rules

- Start by reading `USER_MODEL.md`, `CURRENT_TASK.md`, `GAME_PORTFOLIO.md`, and `CLAUDE.md` or `AGENTS.md`.
- If the task is long, restate the current objective, touched files, untouched files, confirmed specs, and unverified items before making large changes.
- Do not rely on memory when `CURRENT_TASK.md` conflicts with older chat, old code, or old file names. Treat `CURRENT_TASK.md` and Notion/Linear latest records as stronger sources.
- Update or ask to update `CURRENT_TASK.md` when the target game, source file, GAME_SLUG, score contract, public URL, or acceptance criteria changes.
- If context becomes long, produce a `Handoff Summary` before continuing.
- Do not bury user decisions in prose. Separate: AI-decided by harness, user-decision required, and unverified.
- For other AI tools, keep `AGENTS.md` aligned with the same top-level rules as `CLAUDE.md`.
- When adding a new rule, choose the narrowest file: `CLAUDE.md` only for universal short rules, `.claude/rules/` for operational rules, and `docs/harness/` for long procedures.
