# 06_SCORE_RANKING_CONTRACT — ランキングとSupabase契約

Supabaseはランキングとプレイ記録に使う。  
秘密鍵は絶対にブラウザへ入れない。

## 1. ゲーム側固定値

```javascript
const SUPABASE_URL = "https://mlpnjgezrnhdxsxolyzj.supabase.co";
const SUPABASE_PUBLISHABLE_KEY = "ここにPublishable keyを入れる";
const GAME_SLUG = "ここにgame_slug";
const CLIENT_VERSION = "ここにゲーム別バージョン名";
```

Publishable keyはユーザーが手元で差し替える。  
AIチャット、Notion、Linearには貼らない。  
secret keyやservice_role keyは絶対に使わない。

## 2. Supabase `games` に必ず入れる項目

```text
game_slug
title
game_url
description
share_text
score_order
score_unit
score_scale
score_decimals
score_label
first_score_label
best_score_label
is_active
display_order
```

## 3. スコア種別

```text
点数型:
  高いほど良い
  score_order = desc
  score_unit = 点
  score_scale = 1
  score_decimals = 0

短時間型:
  短いほど良い
  score_order = asc
  score_unit = 秒
  score_scale = 100
  score_decimals = 2

長時間型:
  長いほど良い
  score_order = desc
  score_unit = 秒
  score_scale = 100
  score_decimals = 2
```

## 4. 保存値と表示値

Supabaseへ送る `p_score` は整数にする。  
タイム系は、表示秒数を `score_scale` 倍して整数にする。

例:

```text
表示: 34.15秒
score_scale: 100
保存: 3415
```

表示するときは、保存値を `score_scale` で割り戻す。

```text
3415 / 100 = 34.15秒
```

## 5. 使うRPC

```text
submit_score
get_best_score_ranking
get_first_try_ranking
get_game_play_stats
```

古い `get_game_ranking` は使わない。

## 6. スコア送信

結果が確定したら、1回のプレイにつき1回だけ送信する。

```javascript
supabase.rpc("submit_score", {
  p_display_name: playerName,
  p_game_slug: GAME_SLUG,
  p_score: scoreForSubmit,
  p_client_version: CLIENT_VERSION
});
```

結果確定前に送信しない。  
リトライ時に前回送信状態が残らないようにする。

## 7. マイナススコア

マイナススコアがあるゲームでは、0未満を異常扱いしない。

禁止。

```javascript
finalScore = Math.max(0, finalScore);
```

マイナススコアを使う場合は、Score Contractに理論上の下限を明記する。

## 8. 実験場契約

実験場は、Supabase `public.games` を正とする。

### 8.1 自動表示条件

```text
is_active = true
game_slug がある
title がある
game_url がある
display_order がある
```

### 8.2 禁止

```text
ゲーム一覧を固定配列だけで管理する
slice(0, 8) のように件数を固定する
新作ゲームをHTMLに直書きする
ランキングをページ表示時に全件一括取得する
0件をエラー扱いにする
```

### 8.3 ランキング取得

実験場トップでは、ゲーム一覧を表示するために `public.games` を先に取得する。  
ランキング情報は、ページを開いた時点で全ゲーム分をまとめて取得しない。

各ゲームのランキングは、ユーザーがそのゲームのアコーディオンを開いた時に取得する。

```text
get_game_play_stats
get_best_score_ranking
get_first_try_ranking
```

ランキング0件の場合はエラーにしない。  
空表示または `coming soon` 扱いにする。

### 8.4 詳細ランキング

```text
初回記録とベスト記録を分ける
タブで切り替える
横並び表示にしない
記録日時は表示しない
タブ名は first_score_label と best_score_label を使う
保存値ではなく、表示値に変換して出す
```

## 9. localStorage契約

localStorageは便利機能として使う。  
ゲーム本体の成立条件にしない。

守ること。

```text
保存済み名前があれば使う
名前なしでは開始しない
前後の空白を削る
最大20文字
絵文字と記号は原則許可
保存に失敗してもゲーム開始や結果画面を壊さない
旧バージョンの保存データが残っていても落ちない
```

公開済みゲームへランキングを後付けする場合は、旧localStorageデータを読む可能性を必ず考える。  
旧データに新しい `score` がない場合は、旧フィールドから再計算できるなら再計算する。

## 10. ランダム生成契約

ランダム生成を使うゲームでは、将来の検証のために再現できる形にする。

### 10.1 seed

seedとは、同じ乱数結果を再現するための元になる値である。  
以後は「再現用の値」と呼ぶ。

避ける。

```javascript
Math.random();
```

推奨。

```javascript
const rng = createSeededRandom(seed);
const value = rng();
```

### 10.2 将来保存する情報

すぐ全部実装しなくてよい。  
ただし、将来入れられる構造にする。

```text
run_id:
seed:
client_version:
start_time:
end_time:
input_summary:
result_score:
result_reason:
```

### 10.3 ランダム生成の確認項目

```text
絶対にクリア不能な盤面が出ないか
序盤に理不尽配置が出ないか
難易度上昇が急すぎないか
同じ配置が出すぎないか
ランキング上位が運だけにならないか
```

## 11. リプレイ契約

リプレイとは、入力と乱数を記録し、あとから同じプレイを再現する仕組みである。  
以後は「再現プレイ」と呼ぶ。

今すぐ全ゲームに入れなくてよい。  
ただし、以下のゲームでは最初から視野に入れる。

```text
ランキング上位が重要なゲーム
ランダム生成が強いゲーム
高速操作ゲーム
物理挙動があるゲーム
オンライン対戦候補
理論値検証が必要なゲーム
```

将来の保存候補。

```text
run_id
seed
client_version
input_log
result_summary
```
