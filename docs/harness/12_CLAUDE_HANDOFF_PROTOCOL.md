# 12_CLAUDE_HANDOFF_PROTOCOL — 引き継ぎと依頼のプロトコル

## 1. Fable 5向け追加ルール

Fable 5が使える時は、長い作業を任せてよい。  
ただし、長い作業ほど、先に作業単位を分ける。

### 1.1 Fable 5に任せてよいこと

```text
複数ファイルの読み比べ
NotionとLinearの突き合わせ
GitHubコードレビュー
仕様書更新
移行計画作成
TypeScript化の設計
テスト追加
共通部品化の候補出し
```

### 1.2 Fable 5でも禁止

```text
ゲーム性を勝手に変える
スコア計算を勝手に変える
secret keyを扱う
ランキング仕様を曖昧にする
スマホ確認なしで完成扱いにする
旧ファイルを正として扱う
```

## 2. Fable 5が使えない時の縮小運用

Fable 5が使えない時は、作業を小さくする。

```text
1回の依頼で1目的
1回の修正で1画面または1機能
仕様整理と実装を分ける
コードレビューと修正を分ける
Supabase確認を別タスクにする
```

小さいモデルに大きな移行を一気に任せない。  
特にTypeScript化、Three.js化、ランキング修正、実験場修正を同時に頼まない。

## 3. Claude Code依頼テンプレート

### 3.1 新規ゲーム制作依頼

```markdown
あなたは chameleonJP Browser Game Harness に従って作業してください。

最初に読んでください:
- USER_MODEL.md
- CURRENT_TASK.md
- GAME_PORTFOLIO.md
- CLAUDE.md
- docs/harness/03_ACCEPTANCE_GATES.md

今回の目的:
[ここに目的]

対象ゲーム:
[ゲーム名]

必ず守ること:
- スマホ向け
- index.html 1ファイル優先
- 3・2・1カウントダウン
- 結果画面
- ホームシェア
- 結果シェア
- Supabaseランキング前提
- 1プレイ1送信
- Codeberg Pages公開前提

まず、実装前に以下を出してください:
1. ゲームの核
2. 画面状態
3. スコア契約
4. スマホ操作契約
5. Supabase契約
6. 実装リスク
7. 確認方法
```

### 3.2 更新コードレビュー依頼

```markdown
更新されたコードを、chameleonJP Browser Game Harness に従って徹底レビューしてください。

見る範囲:
- 仕様との一致
- スマホ操作
- 結果画面
- Supabaseランキング
- GAME_SLUG
- score_order / score_scale / score_decimals
- シェア文
- Codeberg Pages公開
- secret key混入
- 理不尽挙動
- 処理落ち

出力:
1. 致命的な問題
2. 修正すべき問題
3. 後回しでよい問題
4. よい点
5. ユーザー判断が必要な点
6. 次のClaude Code依頼文
```

### 3.3 修正依頼

```markdown
以下の問題を修正してください。

対象:
[ファイル名]

正とする仕様:
[仕様]

修正してよい範囲:
[範囲]

修正してはいけない範囲:
[範囲]

必ず守ること:
- 結果確定前にスコア送信しない
- 1プレイ1送信
- ホームへ戻る導線を消さない
- Publishable key以外の鍵を入れない
- GAME_SLUGを変えない

出力:
- 変更方針
- 修正版コード
- 確認手順
- 未確認項目
```

### 3.4 移行設計依頼

```markdown
このゲームを将来拡張できる構成へ移行する設計を作ってください。

ただし、すぐ実装しないでください。
まず移行設計だけを出してください。

確認すること:
- 現行仕様
- ゲームの核
- スコア契約
- Supabase契約
- スマホ操作
- Codeberg Pages公開
- 1ファイルHTMLへの戻し方

候補:
- TypeScript
- Vite
- Playwright
- Three.js
- Matter.js
- WebAssembly

出力:
1. 移行するべきか
2. 移行しない方がよい理由があるか
3. 最小移行ステップ
4. 先に入れるテスト
5. ロールバック方法
```

## 4. 移行プレイブック

既存ゲームを新技術へ移す時は、全部作り直さない。

### 4.1 移行順

```text
Step 1:
  現行仕様を固定する

Step 2:
  画面状態、スコア、ランキング、シェアを表にする

Step 3:
  Coreだけを分離する

Step 4:
  Inputを分離する

Step 5:
  Renderを差し替える

Step 6:
  Supabase連携を戻す

Step 7:
  スマホ実機で確認する

Step 8:
  旧版との差分をNotionとLinearに残す
```

### 4.2 失敗扱い

```text
技術移行でゲーム性が変わった
ランキング形式が変わった
スコア単位が変わった
シェア文URLが消えた
スマホ操作が重くなった
Codeberg Pagesで公開できなくなった
```

### 4.3 移行前に固定する表

```markdown
# Migration Freeze

対象ゲーム:
旧版ファイル:
新構成:

## 画面状態

home:
nameConfirm:
countdown:
playing:
result:

## スコア

計算式:
保存値:
表示値:
score_order:
score_scale:
score_decimals:

## ランキング

GAME_SLUG:
CLIENT_VERSION:
RPC:
送信タイミング:

## スマホ操作

主操作:
2本指:
スクロール:
名前入力:

## シェア

ホーム:
結果:

## 移行してはいけない変更

- 
```

## 5. 共通部品化のルール

同じ処理を3ゲーム以上で使ったら、共通化候補にする。

### 5.1 共通化候補

```text
名前入力
3・2・1カウントダウン
結果画面
Supabase送信
ランキング取得
表示値変換
シェア文生成
タッチ抑制
画面サイズ計算
localStorage安全読み書き
seed付き乱数
```

### 5.2 共通化してはいけないもの

```text
ゲームのテンポ
結果コメント
演出の個性
キャラクター性
難易度の味付け
プレイヤーが感じる納得感
```

共通化は、ゲームの個性を消すためではない。  
事故を減らすために行う。

## 6. サブエージェント構成

Fable 5が使える間は、Claudeに複数視点で作業させる。  
Fable 5が使えなくなっても、同じ役割分担を小さなモデルで使えるようにする。

定義は `.claude/agents/` にある。

```text
spec-auditor:              仕様の抜け、矛盾、過去仕様とのズレを見つける
mobile-ux-reviewer:        スマホ操作と画面破綻を見る
ranking-supabase-auditor:  Supabaseとランキングの事故を見る
game-feel-reviewer:        ゲームとして楽しいか、理不尽ではないかを見る
performance-reviewer:      スマホ性能と処理落ちを見る
migration-architect:       将来の技術移行を設計する
release-archivist:         公開と保全の抜けを確認する
security-key-auditor:      鍵と公開情報の事故を防ぐ
```
