# 14_CONTEXT_MEMORY_AND_RULE_LOADING — 文脈・メモリ・ルール読込の扱い

この文書は、Claude側がユーザーを知らない問題を減らすための運用ルールである。  
Claude Codeの `CLAUDE.md` や `.claude/rules/` は強制設定ではなく、作業開始時に読まれる文脈である。  
そのため、重要な情報は短く、具体的に、確認できる形で置く。

---

## 1. 基本方針

```text
CLAUDE.md:
  入口。短く保つ。最重要前提だけを書く。

USER_MODEL.md:
  Claudeがユーザーを知らない問題を補う。

CURRENT_TASK.md:
  今回の作業対象。毎回更新する。

GAME_PORTFOLIO.md:
  作品群の共通性とゲームタイプを理解させる。

docs/harness/:
  長期運用ルール。必要な時に読む。

.claude/rules/:
  Claude Codeが毎回または該当ファイル作業時に見る細かいルール。

.claude/agents/:
  仕様、スマホ、ランキング、性能などの専門レビュー役。
```

---

## 2. 読込順の原則

Claudeまたは他AIには、次の順で読ませる。

```text
1. USER_MODEL.md
2. CURRENT_TASK.md
3. GAME_PORTFOLIO.md
4. CLAUDE.md または AGENTS.md
5. docs/harness/00_OVERVIEW.md
6. docs/harness/03_ACCEPTANCE_GATES.md
7. 今回の作業に関係する docs/harness/*.md
8. 今回の作業に関係する .claude/rules/*.md
```

作業対象がランキングなら、`06_SCORE_RANKING_CONTRACT.md` と `.claude/rules/ranking.md` を読む。  
作業対象がスマホ操作なら、`07_MOBILE_TOUCH_CONTRACT.md` と `.claude/rules/mobile.md` を読む。  
作業対象が技術移行なら、`04_TECH_GROWTH_ROADMAP.md`、`05_TECH_ADOPTION_REVIEW.md`、`12_CLAUDE_HANDOFF_PROTOCOL.md` を読む。

---

## 3. `CLAUDE.md` を大きくしすぎない

`CLAUDE.md` は、毎回の文脈に入る。  
長くしすぎると、他の重要情報を圧迫し、守られにくくなる。

`CLAUDE.md` に置くもの。

```text
最初に読むファイル
最重要前提
絶対禁止
作業姿勢
報告形式
```

`CLAUDE.md` に置かないもの。

```text
長いチェックリスト
ゲームタイプ別の詳細
技術採用審査の全文
Postmortemテンプレート
サブエージェント全文
長い過去経緯
```

長いものは `docs/harness/` へ分ける。

---

## 4. `.claude/rules/` の使い方

ルールファイルは、1テーマ1ファイルにする。

```text
core.md:
  全作業共通

user-context.md:
  ユーザー理解

mobile.md:
  スマホ操作

ranking.md:
  Supabaseとランキング

game-design.md:
  ゲーム性と理不尽さ

performance.md:
  処理落ちと性能

architecture.md:
  構造化と移行

tech-growth.md:
  技術レベルと10年拡張

testing.md:
  手動確認と自動テスト

release.md:
  公開と保全

security.md:
  鍵・外部コード・危険コマンド

context-management.md:
  文脈圧縮、引き継ぎ、CURRENT_TASK更新
```

Claudeが同じ失敗を2回した場合、`CLAUDE.md` へ足す前に、どのルールファイルへ入れるべきか考える。

---

## 5. 作業中に文脈が長くなった時

長いレビューや大規模修正では、途中で文脈が薄くなる。  
その場合、Claudeは作業を続ける前に、次を `CURRENT_TASK.md` または返答へまとめる。

```text
現在の目的
確定した仕様
変更済みファイル
未変更ファイル
既知の問題
次に見るファイル
まだ確認していないこと
ユーザー判断が必要なこと
```

「前に書いたはず」で進めない。  
文脈が長くなったら、明示的に圧縮してから進める。

---

## 6. 引き継ぎメモの型

別AIへ渡す場合、次の形にする。

```markdown
# Handoff Summary

## 対象

ゲーム名:
リポジトリ:
ブランチ:
対象ファイル:

## 現在の目的

## 正とする仕様

## 変更済み

## 未変更

## 重要な制約

- スマホ:
- Supabase:
- GAME_SLUG:
- score_order:
- score_scale:
- シェア文:

## 未確認

## 次にやること

## 触ってはいけないこと
```

---

## 7. `CURRENT_TASK.md` 更新タイミング

次の時は、必ず `CURRENT_TASK.md` を更新する。

```text
対象ゲームが変わった
正とするファイルが変わった
GAME_SLUGが確定した
score_order / score_scale / score_decimalsが確定した
技術レベルを上げる判断をした
公開URLが変わった
Linear IDが変わった
Notionページが変わった
既知の課題が増えた
受け入れ条件が変わった
```

`CURRENT_TASK.md` が古いまま作業しない。  
古い場合は「古い可能性がある」と明記し、NotionやLinearで確認する。

---

## 8. Claude以外のAI向け

Claude以外のAIには `AGENTS.md` を入口にする。  
ただし、内容の正本は `USER_MODEL.md`、`CURRENT_TASK.md`、`docs/harness/` に置く。

`AGENTS.md` は、以下をするための橋渡しである。

```text
Claude Code以外にも読める入口を作る
Codex / ChatGPT / Grok / Gemini に同じ最上位原則を渡す
将来のAIでも、ユーザー理解と品質ゲートへ辿れるようにする
```

---

## 9. ハーネス更新時の注意

ハーネスを更新する時は、以下を確認する。

```text
同じ内容を複数ファイルへ重複させすぎていないか
CLAUDE.mdが肥大化していないか
AGENTS.mdとCLAUDE.mdが矛盾していないか
READMEのファイル構成が古くなっていないか
docs/harness/00_OVERVIEW.md の外部参照が古くなっていないか
新しいルールの置き場所が明確か
```

更新したら `HARNESS_LEARNING.md` または `DECISION_LOG.md` に理由を残す。
