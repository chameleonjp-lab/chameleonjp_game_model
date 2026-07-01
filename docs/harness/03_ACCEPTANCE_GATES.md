# 03_ACCEPTANCE_GATES — 品質ゲート

以下を満たすまで完成扱いにしない。

## 1. 遊びやすさ

```text
最初に何をすればよいか分かる
スタートまでが短い
1プレイの目的が分かる
失敗理由が分かる
もう一度遊びやすい
結果をシェアできる
```

## 2. スマホ

```text
iPhone SE級で遊べる
iOS Safariで初回プレイできる
iOS Safariで2回目以降も遊べる
Androidブラウザでも大きく崩れない
名前入力後に画面がズレない
横スクロールが出ない
ボタンが下部バーに隠れない
タップ位置と当たり判定がズレない
```

## 3. ランキング

```text
Supabase games に登録済み
GAME_SLUG と games.game_slug が一致
submit_score が成功
get_first_try_ranking が取れる
get_best_score_ranking が取れる
get_game_play_stats が取れる
保存値が表示値に正しく直る
0件時に画面が壊れない
送信失敗時も結果画面が壊れない
```

## 4. 公開

```text
Codeberg Pagesで表示できる
index.html が正式名になっている
JavaScript本文が画面に出ていない
<script>タグが壊れていない
シェアURLが正しい
実験場から遊べる
詳細ランキングへの導線が正しい
```

## 5. 保全

```text
Notionに仕様と公開情報を残した
Linearに作業ログと受け入れ条件を残した
Driveに完成版HTMLを保存する方針を確認した
次に別AIが触る時の注意を残した
CURRENT_TASK.md を更新した
```

## 6. Anti-Patterns（避けること）

```text
仕様が曖昧なままコードを書く
スマホ確認を後回しにする
ホーム画面だけきれいで結果画面が弱い
ランキング送信をゲーム中に行う
1プレイで複数回送信する
GAME_SLUG とSupabase登録値がズレる
固定配列だけで実験場のゲーム一覧を管理する
新作ゲームをHTMLへ直書きして増やす
旧コードをコメントアウトで残す
Publishable key以外の鍵を貼る
部分貼り替えでHTMLを壊す
完成後にNotion、Linear、Driveへ残さない
技術移行でゲーム性を変える
新技術を入れること自体を目的にする
```
