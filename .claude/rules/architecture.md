# Architecture Rules

- Current safe standard is index.html single file.
- Even in one file, separate Core, Input, Render, Storage, Ranking, Share, Platform, and Release sections.
- Avoid one function that handles input, score, rendering, and network all together.
- Write functions so future TypeScript/Vite migration is possible.
- Do not introduce frameworks by default.
- New libraries require a technology adoption review.
