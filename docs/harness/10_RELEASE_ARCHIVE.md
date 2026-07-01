# 10_RELEASE_ARCHIVE — 公開と保全

Codebergへ置く正式ファイル名は原則 `index.html`。  
AIが修正版を共有する時は、古い参照を避けるため別名で出す。  
Codebergへ配置する時だけ `index.html` にリネームする。

例:

```text
juden_ga_index_ranked_v20260701_reviewed.html
↓ 公開時
index.html
```

## 1. 完成時にNotionとLinearへ残すこと

```text
仕様書
公開URL
CodebergリポジトリURL
GitHubリポジトリURLがある場合はそのURL
最新ファイル名
Supabase登録値
Publishable key 差し替え行
既知の注意点
実機確認結果
次に別AIが触る時の注意
```

完成版 `index.html` はGoogle Driveにも保管する。  
番号付きファイルは旧版として扱い、番号なしの `index.html` を最新版・確定版として扱う。

## 2. 公開済みゲームの丸ごと差し替え注意

公開済みゲームへ大きな修正を入れる時は、原則として `index.html` を丸ごと差し替える。  
部分貼り替えは避ける。

理由。

```text
古いHTMLと新しいJavaScriptが混ざることがある
<script>開始タグまたは終了タグが欠けると、JavaScript本文が画面に表示される
Publishable keyの問題と、HTML貼り替え破損は別問題
?v=2 や ?v=3 を付けても、ファイル自体が壊れていれば直らない
```

確認すること。

```text
Codeberg上の index.html が最新ファイルと完全一致している
Supabase CDNのscriptタグがある
その直後に自前JavaScriptのscriptタグがある
最後に </script> と </html> がある
Working Copyで部分貼り替えをしていない
```
