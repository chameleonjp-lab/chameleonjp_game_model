# Ranking Rules

- Supabase is the ranking backend.
- GAME_SLUG must match Supabase games.game_slug.
- CLIENT_VERSION must be present.
- p_score must be an integer.
- Use score_order to decide best score.
- Use score_scale and score_decimals for display conversion.
- Use submit_score, get_best_score_ranking, get_first_try_ranking, get_game_play_stats.
- Do not use old get_game_ranking.
- Send once per finalized result.
- If score submission fails, keep the result screen usable.
- Home navigation must remain visible during ranking submission.
- Do not treat negative scores as invalid unless the game contract forbids them.
