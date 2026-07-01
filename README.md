# chameleonJP Browser Game Harness

カメレオンJPのスマホ向けブラウザゲームを、AI（Claude / Claude Code / Fable 5 / Codex / ChatGPT / 未来のAI）に引き継ぎながら10年作り続けるための統合ハーネスです。

出典: `chameleonJP Browser Game Harness v5.0`（2026-07-01作成）  
原本の単一Markdown版は [`docs/harness/chameleonjp_browser_game_harness_v5.md`](docs/harness/chameleonjp_browser_game_harness_v5.md) に保全しています。

## このリポジトリは何か

ハーネスとは、次の4つをまとめた「作業の型」です。

1. AIが作業前に読む前提
2. AIが守るべき制作ルール
3. AIが作業中に確認する品質ゲート
4. AIが作業後に残す引き継ぎ情報

目的:

```text
1. 今の安全な開発運用を守る
2. ユーザーの10年分のスキルアップに合わせて拡張できる土台を作る
3. Fable 5が使えなくなっても、別モデルで同じ判断に近づける
```

## AIが最初に読む順番

1. [`USER_MODEL.md`](USER_MODEL.md) — ユーザー理解（非エンジニアだが要求は厳密）
2. [`CURRENT_TASK.md`](CURRENT_TASK.md) — 今回の作業対象と契約（作業ごとに更新する）
3. [`GAME_PORTFOLIO.md`](GAME_PORTFOLIO.md) — 作品群の一覧と共通性
4. [`CLAUDE.md`](CLAUDE.md) — Claude Codeの入口（最重要前提の要約）
5. [`docs/harness/00_OVERVIEW.md`](docs/harness/00_OVERVIEW.md) — ハーネス全体像・最上位原則・作業フロー
6. [`docs/harness/03_ACCEPTANCE_GATES.md`](docs/harness/03_ACCEPTANCE_GATES.md) — 完成扱いにするための品質ゲート

## ファイル構成

```text
/
├─ README.md                  ← このファイル
├─ CLAUDE.md                  ← Claude Codeの入口（短く保つ）
├─ CURRENT_TASK.md            ← 現在作業のテンプレート（作業ごとに記入）
├─ USER_MODEL.md              ← ユーザーモデル（好み・嫌う失敗・依頼文の読み方）
├─ GAME_PORTFOLIO.md          ← ゲーム一覧（公開済み / 要確認 / 構想）
├─ DECISION_LOG.md            ← 技術選定・仕様変更・例外判断の記録
├─ HARNESS_LEARNING.md        ← 同じ失敗を2回したら追加する学習ログ
├─ docs/
│  └─ harness/
│     ├─ 00_OVERVIEW.md                 ← 全体像・最上位原則・情報源の優先順位・作業フロー
│     ├─ 01_USER_MODEL_GUIDE.md         ← ユーザーモデルの詳細ガイド
│     ├─ 02_GAME_SPEC_TEMPLATE.md       ← ゲーム契約テンプレート・画面状態契約・タイプ別の見るべき点
│     ├─ 03_ACCEPTANCE_GATES.md         ← 品質ゲートとAnti-Patterns
│     ├─ 04_TECH_GROWTH_ROADMAP.md      ← 技術レベル（Level 0〜8）・技術レーダー・10年ロードマップ
│     ├─ 05_TECH_ADOPTION_REVIEW.md     ← 技術採用審査テンプレート・アーキテクチャ原則
│     ├─ 06_SCORE_RANKING_CONTRACT.md   ← Supabase・ランキング・実験場・localStorage・乱数・リプレイ契約
│     ├─ 07_MOBILE_TOUCH_CONTRACT.md    ← スマホ操作契約（対象端末・スクロール・タッチ抑制）
│     ├─ 08_PERFORMANCE_BUDGET.md       ← パフォーマンス予算
│     ├─ 09_TESTING_STRATEGY.md         ← テスト戦略（手動 / ロジック / Playwright）
│     ├─ 10_RELEASE_ARCHIVE.md          ← 公開と保全（Codeberg Pages / Notion / Linear / Drive）
│     ├─ 11_POSTMORTEM_TEMPLATE.md      ← 公開後のPostmortemテンプレート
│     ├─ 12_CLAUDE_HANDOFF_PROTOCOL.md  ← 依頼テンプレート・移行プレイブック・共通部品化ルール
│     └─ chameleonjp_browser_game_harness_v5.md  ← 原本（単一Markdown版・v5.0全文）
└─ .claude/
   ├─ rules/                 ← Claude Codeが常に守るルール
   │  ├─ core.md
   │  ├─ user-context.md
   │  ├─ mobile.md
   │  ├─ ranking.md
   │  ├─ game-design.md
   │  ├─ performance.md
   │  ├─ architecture.md
   │  ├─ tech-growth.md
   │  ├─ testing.md
   │  └─ release.md
   └─ agents/                ← レビュー用サブエージェント定義
      ├─ spec-auditor.md              ← 仕様の抜け・矛盾・ズレ
      ├─ mobile-ux-reviewer.md        ← スマホ操作と画面破綻
      ├─ ranking-supabase-auditor.md  ← Supabaseとランキング事故
      ├─ game-feel-reviewer.md        ← 楽しさ・理不尽さ
      ├─ performance-reviewer.md      ← スマホ性能・処理落ち
      ├─ migration-architect.md       ← 将来の技術移行設計
      ├─ release-archivist.md         ← 公開と保全の抜け
      └─ security-key-auditor.md      ← 鍵と公開情報の事故防止
```

## 絶対に守る最上位原則（要約）

技術の新しさよりも、スマホで遊べること、ランキングが壊れないこと、公開後に復旧できることを優先する。

```text
スマホで遊びやすい
最初に何をすればいいか分かる
1プレイが短く、もう一度遊びたくなる
名前入力が壊れない
3・2・1カウントダウンがある
結果画面が壊れない
今回記録、初回記録、ベスト記録、プレイ回数が出る
ランキング送信は結果確定後に1回だけ
シェア文の末尾にゲームURLがある
GAME_SLUG と Supabase games.game_slug が一致している
Codeberg Pagesで公開できる
Notion、Linear、Google Driveへ保全できる
secret key / service_role key を扱わない
```

全文は [`docs/harness/00_OVERVIEW.md`](docs/harness/00_OVERVIEW.md) を参照。

## 使い方

### 新しい作業を始める時

1. `CURRENT_TASK.md` に今回の対象・契約・受け入れ条件を記入する
2. `docs/harness/12_CLAUDE_HANDOFF_PROTOCOL.md` の依頼テンプレートを使ってAIに依頼する
3. AIは `USER_MODEL.md` → `CURRENT_TASK.md` → `GAME_PORTFOLIO.md` → `CLAUDE.md` の順に読んでから作業する

### 作業が終わった時

1. `docs/harness/03_ACCEPTANCE_GATES.md` の品質ゲートを満たしているか確認する
2. 判断を `DECISION_LOG.md` へ、失敗の学びを `HARNESS_LEARNING.md` へ残す
3. Notion / Linear / Google Drive へ保全する（`docs/harness/10_RELEASE_ARCHIVE.md` 参照）

### ハーネス自体を更新する時

`docs/harness/00_OVERVIEW.md` の「ハーネスの更新ルール」に従う。  
同じ失敗を2回したら、必ず `HARNESS_LEARNING.md` と該当ルールファイルへ反映する。

## 関連する外部の正式情報源

```text
Notion:        🎮 ブラウザゲーム(chameleonJP) / 🧩 共通仕様マスト / 🎮 個別ゲーム索引
Linear:        CHA- プレフィックスタスク
Google Drive:  完成版HTMLの保全場所
Codeberg:      公開元（Codeberg Pages）
Supabase:      public.games / submit_score / get_best_score_ranking / get_first_try_ranking / get_game_play_stats
GitHub:        chameleonjp-lab（AI作業、レビュー、保全、将来のCI候補）
```

優先順位は「現在のユーザー指示 > Notion新ハブ > 共通仕様マスト > 個別ゲーム仕様 > Linear最新ログ > 最新コード > 過去会話 > 古いファイル」。

## セキュリティ

- Supabaseの **Publishable key以外の鍵**（secret key / service_role key）を、コード・チャット・Notion・Linearに絶対に入れない。
- Publishable keyはユーザーが手元で差し替える。差し替え行は必ず明示する。
