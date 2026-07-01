# DECISION_LOG.md

## 記録ルール

技術選定、仕様変更、ランキング設定、例外判断はここに残す。  
理由が残っていない変更は、将来の復旧時に事故の原因になる。

---

## Decision 001

日付: 2026-07-01  
対象ゲーム: ハーネス本体  
対象ファイル: `AGENTS.md`, `.gitignore`, `docs/harness/13_SECURITY_AND_AGENT_SAFETY.md`, `docs/harness/14_CONTEXT_MEMORY_AND_RULE_LOADING.md`, `docs/harness/15_REPO_BOOTSTRAP_CHECKLIST.md`, `.claude/rules/security.md`, `.claude/rules/context-management.md`, `.claude/agents/repo-safety-auditor.md`, `.claude/agents/context-handoff-auditor.md`, `README.md`  
判断したこと: Claude以外のAI向け入口、外部コード安全確認、長い作業の引き継ぎ、リポジトリ初期化チェックを追加した。

選んだ案:  
既存の `CLAUDE.md` を巨大化させず、`AGENTS.md` と `docs/harness/` と `.claude/rules/` と `.claude/agents/` に分ける。

選ばなかった案:  
`CLAUDE.md` へすべて追記する。  
理由は、入口ファイルが長くなりすぎると、Claudeが毎回読む文脈が重くなり、守るべき条件が埋もれるため。

理由:  
このハーネスはClaudeだけでなく、Codex、ChatGPT、Grok、Gemini、未来のAIにも渡す想定がある。さらに、外部リポジトリやセットアップ手順をAIが扱う時、安全確認が弱いと事故が起きる可能性がある。そこで、Claude向け入口と他AI向け入口を分け、外部コード確認と引き継ぎ整理を専用化した。

影響:
- ゲーム性: 直接変更なし。ゲームの核を守る前提を維持。
- スマホ操作: 直接変更なし。既存のスマホ契約を維持。
- ランキング: 直接変更なし。既存のSupabase契約を維持。
- Supabase: 公開してはいけない値を扱わないルールを強化。
- 公開: 新規ゲームリポジトリ初期化時の不足を減らす。
- 将来移行: Claude以外のAIと長文脈の引き継ぎに強くなる。

Notionへ残したか: 未実施  
Linearへ残したか: 未実施
