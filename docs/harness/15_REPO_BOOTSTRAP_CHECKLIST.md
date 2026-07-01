# 15_REPO_BOOTSTRAP_CHECKLIST — 新規ゲームリポジトリ初期化チェック

この文書は、新しいゲームリポジトリを作る時、または既存リポジトリをAI作業可能な状態へ整える時のチェックリストである。

---

## 1. 最初に置くファイル

新規ゲームリポジトリには、最低限次を置く。

```text
README.md
CLAUDE.md
AGENTS.md
CURRENT_TASK.md
USER_MODEL.md または chameleonjp_game_model への参照
GAME_SPEC.md
DECISION_LOG.md
HARNESS_LEARNING.md
.gitignore
index.html
```

ハーネス本体を丸ごとコピーしない場合でも、`CLAUDE.md` と `AGENTS.md` から `chameleonjp_game_model` へ辿れるようにする。

---

## 2. `README.md` に書くこと

```text
ゲーム名
短い説明
公開URL
CodebergリポジトリURL
GitHubリポジトリURL
技術レベル
ファイル構成
Supabase連携の有無
Publishable key差し替え行
スマホ確認対象
Notionページ
Linear ID
```

---

## 3. `CURRENT_TASK.md` に書くこと

```text
対象ゲーム
正とする仕様
正とするファイル
今回やること
今回やらないこと
GAME_SLUG
CLIENT_VERSION
score_order
score_scale
score_decimals
スマホ操作契約
受け入れ条件
未確認事項
```

`CURRENT_TASK.md` が空のままAIに作業させない。

---

## 4. `GAME_SPEC.md` に書くこと

```text
ゲームの核
1プレイの長さ
操作方法
成功条件
失敗条件
終了条件
画面状態
スコア計算
ランキング種別
シェア文
2本指操作
理不尽防止
```

仕様がNotionにある場合も、リポジトリ内に要約を置く。  
ClaudeがNotionへ接続できない時でも、最低限の作業ができるようにするためである。

---

## 5. `index.html` の最低条件

```text
<!doctype html> から始まる
HTML / CSS / JavaScriptが壊れていない
scriptタグが閉じている
GAME_SLUGがある
CLIENT_VERSIONがある
Publishable key差し替え行が分かる
ホーム画面がある
名前確認がある
3・2・1カウントダウンがある
結果画面がある
ホームシェアがある
結果シェアがある
```

公開済みゲームの大きな修正では、原則として `index.html` を丸ごと差し替える。  
部分貼り替えで `<script>` タグを壊す事故を避ける。

---

## 6. `.gitignore` の最低条件

```text
CLAUDE.local.md
.env
.env.*
node_modules/
dist/
build/
coverage/
playwright-report/
test-results/
*.secret
*service_role*
*supabase_key*
*.key
*.pem
```

ゲームリポジトリで `dist/` をCodeberg Pages公開物としてコミットする必要がある場合は、個別に理由を `DECISION_LOG.md` へ残して例外にする。

---

## 7. Supabase登録前チェック

```text
GAME_SLUGが確定している
公開URLが確定している
titleが確定している
descriptionが確定している
share_textが確定している
score_orderが確定している
score_unitが確定している
score_scaleが確定している
score_decimalsが確定している
score_labelが確定している
first_score_labelが確定している
best_score_labelが確定している
display_orderが決まっている
is_activeをどうするか決まっている
```

登録値はNotionとLinearへ残す。  
secret keyは残さない。

---

## 8. 公開前チェック

```text
スマホでホームが見える
名前入力後にblurする
カウントダウン中にゲーム対象が見えない
プレイできる
結果画面に今回記録が出る
初回記録が出る
ベスト記録が出る
プレイ回数が出る
送信失敗時も結果画面が壊れない
ホームへ戻れる
もう一度遊べる
シェア文末尾にURLがある
横スクロールがない
iPhone SE級でボタンが隠れない
Codeberg Pagesで表示できる
```

---

## 9. 公開後に残すこと

```text
Notion:
  仕様書
  公開URL
  リポジトリURL
  Supabase登録値
  最新ファイル名
  注意点
  実機確認結果

Linear:
  作業ログ
  受け入れ条件
  未確認項目
  次にAIが触る時の注意

Google Drive:
  完成版 index.html
  必要なら関連アセット

GitHub / Codeberg:
  公開状態
  README更新
```

---

## 10. 初期化完了の合格条件

```text
AIが USER_MODEL / CURRENT_TASK / GAME_SPEC を読めば作業できる
secret keyが混入していない
スマホ確認項目が明記されている
Supabase契約が明記されている
公開URLとGAME_SLUGが矛盾していない
Notion / Linear / Drive の保全先が分かる
```
