# Performance Rules

- Target 60fps when possible.
- Heavy games may accept stable 30fps.
- Use requestAnimationFrame for game loops.
- Stop loops outside playing state.
- Do not recreate Canvas every frame.
- Do not update large DOM sections every frame.
- Avoid large object allocation inside hot loops.
- Put caps on particles, enemies, projectiles, and effects.
- Do not make Supabase calls during active gameplay.
- Do not recalculate layout every frame.
