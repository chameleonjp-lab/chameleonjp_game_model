# 00_OVERVIEW — chameleonJP Browser Game Harness 全体像

出典: chameleonJP Browser Game Harness v5.0（2026-07-01）

## 1. このハーネスの位置づけ

この文書群は、単なるプロンプトではない。  
カメレオンJPのブラウザゲームを、今後10年作り続けるための「作業の型」である。

ハーネスとは、次の4つをまとめたものを指す。

1. AIが作業前に読む前提
2. AIが守るべき制作ルール
3. AIが作業中に確認する品質ゲート
4. AIが作業後に残す引き継ぎ情報

目的は次の3つ。

```text
1. 今の安全な開発運用を守る
2. ユーザーの10年分のスキルアップに合わせて拡張できる土台を作る
3. Fable 5が使えなくなっても、別モデルで同じ判断に近づける
```

## 2. 絶対に守る最上位原則

このプロジェクトでは、技術の新しさよりも、スマホで遊べること、ランキングが壊れないこと、公開後に復旧できることを優先する。

次は、どのゲームでも、どの技術でも、どのAIでも守る。

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

技術的に正しくても、スマホで遊びにくいものは失敗とする。  
見た目がよくても、ランキング、結果画面、シェア、公開導線が壊れているものは完成扱いにしない。

## 3. 正式情報源と優先順位

### 3.1 正式情報源

作業開始時は、次を確認する。

```text
Notion:
  🎮 ブラウザゲーム(chameleonJP)
  🧩 共通仕様マスト
  🎮 個別ゲーム索引
  AI対応メモ｜ユーザーの好みと進め方（ブラウザゲーム）

Linear:
  CHA- プレフィックスタスク

Google Drive:
  完成版HTMLの保全場所

Codeberg:
  公開元
  Codeberg Pages

Supabase:
  public.games
  submit_score
  get_best_score_ranking
  get_first_try_ranking
  get_game_play_stats

GitHub:
  chameleonjp-lab
  AI作業、レビュー、保全、将来のCI候補
```

### 3.2 優先順位

```text
1. 現在のユーザー指示
2. Notionの新ハブ「ブラウザゲーム(chameleonJP)」
3. 共通仕様マスト
4. 個別ゲーム仕様
5. Linearの最新作業ログ
6. 最新コード
7. 過去会話
8. 古い仕様書・古い修正版ファイル
```

旧ハブや古いページは、正式情報源として扱わない。  
個別ゲーム仕様と共通仕様が矛盾する場合は、原則として共通仕様を優先する。  
例外にする場合は、理由をNotionとLinearに残す。

### 3.3 注意が必要な表記ゆれ

過去に `anatano_1byou` と `anatano_1byo` のようなslug表記ゆれがあった。  
この種類の表記ゆれは、実験場、詳細ランキング、Supabase登録、公開URLを壊す。

Claudeは、slugを推測しない。  
必ず以下で確認する。

```text
ゲーム側定数 GAME_SLUG
Supabase public.games.game_slug
公開URL
実験場リンク
詳細ランキングURLの game パラメータ
Notionの最新仕様
Linearの最新ログ
```

## 4. ゲーム制作思想

### 4.1 小さくても雑にしない

カメレオンJPのゲームは、短時間で遊ぶスマホブラウザゲームが中心である。  
ただし、短いゲームだから雑でよいわけではない。

重要なのは、次の流れである。

```text
すぐ分かる
すぐ触れる
失敗理由が分かる
結果が出る
ランキングに残る
シェアできる
もう一度遊べる
```

### 4.2 参考ゲームの扱い

既存ゲームを参考にする時は、最初から無理にオリジナル化してロジックを壊さない。  
まず参考元の面白さの核を理解し、成立する最小構造を作る。  
その後で、テーマ、見た目、演出、キャラクター、難易度、シェア文、ランキングをカメレオンJP向けに変える。

これは著作権侵害を推奨する意味ではない。  
「ゲームとして成立する構造を正確に理解してから変える」という意味である。

### 4.3 技術よりゲーム体験が先

Claudeは、毎回この順番で判断する。

```text
1. ゲームの核は何か
2. スマホでその核が気持ちよく触れるか
3. 結果画面とランキングが壊れないか
4. Supabase登録値とゲーム側定数が一致するか
5. Codeberg Pagesで公開できるか
6. ユーザーがWorking Copyで扱えるか
7. Notion/Linear/Driveへ保全できるか
8. 将来の技術移行を邪魔しないか
```

## 5. 作業フロー

Claudeは、毎回この順で動く。

### 5.1 読む

```text
CURRENT_TASK.md
USER_MODEL.md
GAME_PORTFOLIO.md
CLAUDE.md
docs/harness/該当ファイル
.claude/rules/該当ファイル
Notion
Linear
最新コード
```

### 5.2 分ける

```text
今回やること
今回やらないこと
触るファイル
触らないファイル
壊してはいけない契約
未確認の項目
```

### 5.3 実装前に出す

```text
変更方針
リスク
確認方法
ロールバック方法
```

### 5.4 実装前にClaudeが必ず言語化すること

```text
今回の目的
触るファイル
触らないファイル
壊してはいけない仕様
リスク
確認方法
未確認になりそうなこと
```

これを書けない状態で実装しない。

### 5.5 実装後にClaudeが必ず報告すること

```text
変更した内容
変更していない内容
確認した内容
確認できていない内容
スマホで見るべき点
Supabaseで見るべき点
Notion/Linear/Driveへ残す内容
次の修正候補
```

「完了しました」だけは禁止。

## 6. 最終判断基準

Claudeが迷ったら、次の順で判断する。

```text
1. ユーザーのゲーム制作思想に合うか
2. スマホで遊びやすいか
3. ゲームの核が強くなるか
4. ランキングと結果画面が壊れないか
5. Codeberg Pagesで公開できるか
6. ユーザーがWorking Copyで扱えるか
7. Notion/Linear/Driveへ保全できるか
8. 将来のTypeScript化やWebGL化を邪魔しないか
9. 今その技術を入れる必要があるか
```

新技術は目的ではない。  
ゲームを良くするため、事故を減らすため、長く運用するためにだけ使う。

## 7. ハーネスの更新ルール

このハーネスは固定物ではない。  
ただし、思いつきで膨らませない。

更新する条件。

```text
同じ失敗を2回した
新しいゲームタイプが増えた
Supabase仕様が変わった
Codeberg Pagesの公開手順が変わった
Claude Codeの記憶・rules・agents仕様が変わった
ユーザーの開発スキルが上がり、標準Levelを上げる必要が出た
共通部品が3ゲーム以上で使われた
```

更新時に必ず書く。

```text
何を追加したか
なぜ追加したか
どの失敗を防ぐか
どのファイルへ分けるべきか
古いルールと矛盾しないか
```

## 8. 外部仕様参照

ハーネスを更新する時の参照先。  
この章だけを根拠に決めず、最新の公式ドキュメントを確認する。

```text
Claude Code Memory:
https://code.claude.com/docs/en/memory

Claude Code Subagents:
https://code.claude.com/docs/en/sub-agents

Supabase API Keys:
https://supabase.com/docs/guides/getting-started/api-keys

Codeberg Pages:
https://docs.codeberg.org/codeberg-pages/

TypeScript Handbook:
https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html

Vite Guide:
https://vite.dev/guide/

Playwright:
https://playwright.dev/docs/intro

MDN WebGL:
https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API

MDN WebAssembly:
https://developer.mozilla.org/en-US/docs/WebAssembly/Guides/Concepts
```
