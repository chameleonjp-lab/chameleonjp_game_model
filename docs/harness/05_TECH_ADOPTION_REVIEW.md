# 05_TECH_ADOPTION_REVIEW — 技術採用審査テンプレート

新しい技術、言語、ライブラリ、ビルド環境を入れる時は、必ずこれを書く。

```markdown
# 技術採用審査

## 候補

技術名:

## 採用理由

今困っていること:

今の標準では足りない理由:

## ゲーム体験への効果

良くなる点:

悪くなる可能性:

## スマホ性能

iPhone 17 Pro:
iPhone 11 Pro:
iPhone SE級:
Android:

## 運用への影響

Working Copyで扱えるか:
Codeberg Pagesで公開できるか:
Driveに完成版を残せるか:
Notion/Linearへ説明できるか:

## Supabaseへの影響

GAME_SLUG:
submit_score:
score_order:
score_scale:
ランキング表示:

## 戻せるか

1ファイルHTMLへ戻せるか:
戻す場合の手順:

## 不採用にした代替案

代替案1:
代替案2:

## 判断

採用 / 試験採用 / 保留 / 不採用

理由:
```

1つでも答えられない場合は、採用を急がない。

## アーキテクチャ原則

1ファイルでも、頭の中では層を分ける。  
将来TypeScriptや複数ファイルに移す時、この層ごとに分ける。

```text
Game Core:
  ルール、勝敗、スコア、難易度、ランダム生成

Input:
  タップ、スワイプ、ドラッグ、キーボード、2本指操作

Render:
  DOM、Canvas 2D、WebGL、Three.js

Platform:
  iPhone Safari、Android、画面サイズ、safe-area、visualViewport

Storage:
  localStorage、旧データ移行

Ranking:
  Supabase送信、ランキング取得、表示値変換

Share:
  ホームシェア、結果シェア

Release:
  Codeberg Pages、Drive保全、Notion/Linear記録
```

### 禁止する混在

悪い例。

```javascript
function update() {
  // 入力、描画、スコア計算、Supabase送信、画面遷移を全部やる
}
```

良い例。

```javascript
readInput();
updateGameState();
resolveCollisions();
calculateScore();
renderFrame();
```

1ファイルでも、関数名で役割を分ける。
