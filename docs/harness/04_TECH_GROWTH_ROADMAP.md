# 04_TECH_GROWTH_ROADMAP — 技術レベルと10年ロードマップ

## 1. 技術レベル

技術を「今はこれだけ」と固定しない。  
ただし、毎回なんとなく新技術を入れない。

ゲームごとに、最初に技術レベルを決める。

```text
Level 0: index.html 1ファイル + Vanilla JavaScript
Level 1: index.html 1ファイル + 内部構造を分割したJavaScript
Level 2: 複数ファイルJavaScript + 公開時に静的ファイル化
Level 3: TypeScript + Vite
Level 4: Canvas 2D中心 + 自動テスト
Level 5: WebGL / Three.js / Matter.js / Phaser
Level 6: Rust / WebAssembly / 高度な生成処理
Level 7: オンライン同期 / リプレイ検証 / サーバー側検証
Level 8: 共通SDK化 / 複数ゲーム横断基盤
```

### 1.1 現在の標準

現在の標準はLevel 0〜1。

```text
index.html 1ファイル
HTML/CSS/JavaScript統合
Canvas 2D または軽量DOM
Supabaseランキング
Codeberg Pages公開
Working Copyで編集・push
Notion/Linear/Drive保全
```

これは「今の安全な標準」である。  
ただし「10年後の上限」ではない。

### 1.2 上のLevelへ進む条件

Levelを上げる時は、次のどれかが必要。

```text
ゲームロジックが複雑になり、1ファイルでは壊れやすい
カード、敵、ステージ、状態異常などの種類が多い
スコア計算やランキング表示の事故を減らしたい
ランダム生成の検証が必要
3D、物理、奥行き表現がゲームの核になる
実機で性能が足りない
自動テストがないと修正事故が増えている
```

「新しい技術を使いたいから」だけでは上げない。

## 2. 技術レーダー

技術は4段階で管理する。

```text
Adopt: 今の標準として使う
Trial: 一部ゲームで試す
Assess: 調査するが本番ではまだ使わない
Hold: 今は使わない
```

### 2.1 現在の技術レーダー

```text
Adopt:
  HTML
  CSS
  Vanilla JavaScript
  Canvas 2D
  Supabase RPC
  Codeberg Pages
  Notion
  Linear
  Google Drive
  Working Copy

Trial:
  TypeScript
  Vite
  Playwright
  Vitest
  Matter.js
  Three.js

Assess:
  Phaser
  WebAssembly
  Rust
  WebGPU
  Service Worker
  IndexedDB
  Web Audio API

Hold:
  React標準化
  Next.js標準化
  大型ゲームエンジン標準化
  サーバー必須構成
  認証必須のランキング
```

### 2.2 TypeScriptの扱い

TypeScriptは、JavaScriptに型を足して、実行前に一部の間違いを見つけるための候補。  
ゲーム設定、状態、スコア計算、Supabase返却値が増えた時に検討する。

使う条件。

```text
状態が複雑
敵・カード・アイテムが多い
ランキング値の型事故が起きやすい
長期運用する
自動テストを入れる
```

### 2.3 Viteの扱い

Viteは、開発中に複数ファイルを扱いやすくし、公開時に静的ファイルを出すための候補。

使う条件。

```text
index.html 1ファイルが長くなりすぎた
共通部品を分けたい
TypeScriptを使いたい
自動テストやプレビューを使いたい
```

公開時は、Codeberg Pagesで動く静的ファイルにすること。  
「ビルドしないと動かないが、ユーザーが扱えない」状態にはしない。

### 2.4 Playwrightの扱い

Playwrightは、ブラウザ操作を自動確認するための候補。  
ホーム、名前入力、カウントダウン、結果画面、ランキング失敗時の導線などを確認する。

使う条件。

```text
公開済みゲームの修正事故を減らしたい
結果画面や名前入力の退行を防ぎたい
複数端末の画面確認を型にしたい
```

### 2.5 WebGL / Three.jsの扱い

WebGLは、ブラウザで高性能な2D/3D描画を行うための候補。  
Three.jsは、WebGLを直接書かずに3D表現を扱いやすくする候補。

使う条件。

```text
奥行きがゲームの核
3D回転が必要
Canvas 2Dでは表現しにくい
見た目の価値がゲーム体験に直結する
```

全ゲーム標準にしない。  
まず1本で試す。

### 2.6 Matter.jsの扱い

Matter.jsは、物理挙動を扱うための候補。  
落下、衝突、反発、押し出しがゲームの核なら検討する。

使う条件。

```text
手書き当たり判定では納得感が出ない
物理挙動がゲームの面白さになる
回避不能や不自然な衝突を減らしたい
```

### 2.7 WebAssembly / Rustの扱い

WebAssemblyは、JavaScriptの置き換えではない。  
重い計算だけを補う候補として扱う。

使う条件。

```text
盤面生成が重い
解ける問題の検証が必要
大量の敵や弾を処理する
リプレイ検証が必要
不正検知が必要
```

ゲーム全体をいきなりRustへ移さない。  
まず重い計算だけを切り出す。

## 3. 10年ロードマップ

### 3.1 0〜1年

目的: 今の運用を壊さず、完成率を上げる。

```text
index.html 1ファイル
Canvas 2D
スマホ操作
Supabaseランキング
実験場連携
Notion/Linear/Drive保全
```

### 3.2 1〜3年

目的: 事故を減らす。

```text
共通テンプレート
共通ランキング処理
共通結果画面
seed付き乱数
軽い自動テスト
TypeScript試験採用
Vite試験採用
```

### 3.3 3〜5年

目的: 表現力を上げる。

```text
Three.js
Matter.js
WebGL
音管理
アニメーション管理
リプレイ検証
性能計測
```

### 3.4 5〜10年

目的: ゲーム群を基盤化する。

```text
共通SDK
TypeScript標準化
自動テスト標準化
不正検知
ランキング監査
オンライン同期
WebAssembly
高度な盤面生成
```

ただし、10年後も「スマホで遊べる」「ランキングが壊れない」「公開できる」を最優先にする。
