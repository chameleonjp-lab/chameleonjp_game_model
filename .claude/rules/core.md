# Core Rules

- Read USER_MODEL.md and CURRENT_TASK.md before changing code.
- Do not treat the user as a generic beginner. Explain simply, but reason strictly.
- Preserve the game core unless the user explicitly asks to change it.
- Separate home, nameConfirm, countdown, playing, and result states.
- Do not send score before result is final.
- Do not call submit_score more than once per play.
- Do not remove home share or result share unless explicitly approved.
- Do not remove home navigation from the result screen.
- Do not use old files as source of truth when CURRENT_TASK says otherwise.
- Report touched files, untouched files, verified items, and unverified items.
