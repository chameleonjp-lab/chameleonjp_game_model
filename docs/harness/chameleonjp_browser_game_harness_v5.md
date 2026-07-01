# chameleonJP Browser Game Harness v5.0

作成日: 2026-07-01  
用途: Claude / Claude Code / Fable 5 / Codex / ChatGPT / 未来のAIに、カメレオンJPのブラウザゲーム開発を引き継がせるための統合ハーネス。  
状態: 単一Markdown版。将来はこのファイルを `.claude/rules/`、`.claude/agents/`、`docs/harness/` へ分割して使う。

---

## 0. この文書の位置づけ

この文書は、単なるプロンプトではない。  
カメレオンJPのブラウザゲームを、今後10年作り続けるための「作業の型」である。

ここで言うハーネスとは、次の4つをまとめたものを指す。

1. AIが作業前に読む前提
2. AIが守るべき制作ルール
3. AIが作業中に確認する品質ゲート
4. AIが作業後に残す引き継ぎ情報

Claude側は、ChatGPTほどユーザーの過去文脈を知らない。  
そのため、このハーネスには「ゲーム開発のルール」だけでなく、「ユーザーをどう理解するか」も含める。

この文書の目的は、次の3つである。

```text
1. 今の安全な開発運用を守る
2. ユーザーの10年分のスキルアップに合わせて拡張できる土台を作る
3. Fable 5が使えなくなっても、別モデルで同じ判断に近づける
```

---

## 1. 絶対に守る最上位原則

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

---

## 2. Claudeに理解させるユーザーモデル

Claudeは、ユーザーを「普通の初心者」として扱ってはいけない。  
ユーザーは非エンジニアだが、AIを使って多数のスマホ向けブラウザゲームを作り、ランキング連携、公開、保全まで行う制作者である。

### 2.1 ユーザーの基本像

```text
ユーザー:
  非エンジニア。
  コードを一から手書きするより、AIに仕様書、実装計画、コード、レビュー、修正依頼文を作らせる。
  スマホ実機でプレイし、違和感、理不尽さ、UIの弱さ、ランキング事故を見つける。

得意:
  ゲーム体験の違和感を見つける
  仕様の抜けに気づく
  スマホ操作の悪さを判断する
  ランキングや結果画面の破綻を見つける
  AIへの指示を改善していく

現在の主な作業環境:
  iPhone 17 Pro
  iPhone 11 Pro
  iPad Pro
  Working Copy
  GitHub chameleonjp-lab
  Codeberg Pages
  Supabase
  Notion
  Linear
  Google Drive
```

### 2.2 ユーザーが求める説明

ユーザー向けの説明は、やさしく、具体的にする。  
ただし、仕様判断は甘くしない。

必ず明示する。

```text
どのファイルを触るか
どこを差し替えるか
丸ごと差し替えか、部分修正か
公開時に index.html へリネームする必要があるか
Supabase Publishable key を入れる行
作業後にスマホで確認する項目
未確認のまま残ること
Notion / Linear / Drive に残すこと
```

専門語を使う場合は、先にやさしい説明を1文置く。  
例:

```text
同じ乱数結果をあとから再現できる値を持たせます。
これは seed と呼ばれます。
以後は「再現用の値」と書きます。
```

### 2.3 ユーザーが嫌う失敗

Claudeは、次を特に避ける。

```text
仕様を勝手に変える
ゲーム性を丸める
スマホ操作を後回しにする
ランキングを最後に雑に足す
Supabaseのslug、score_scale、score_orderを間違える
結果画面の導線を消す
シェア文のURLを消す
古いファイルを最新版として扱う
「技術的には正しいが、遊ぶとつまらない」実装にする
「たぶん大丈夫」で完成扱いにする
JavaScript本文が画面に出るような貼り替え事故を起こす
Publishable key以外の鍵を扱う
```

### 2.4 ユーザーの依頼文の読み方

#### 「更新されました」

最新コードが変わったという意味。  
Claudeは、差分だけでなく、仕様、ゲームロジック、スマホ操作、結果画面、ランキング、シェア、公開導線を確認する。

#### 「徹底レビュー」

表面的なコードレビューではない。  
次を含めて見る。

```text
ゲームロジック
スマホ操作
UI/UX
ランキング
Supabase
結果画面
シェア
理不尽さ
理論値
不正っぽいスコア
Codeberg Pages公開
Notion/Linear保全
```

#### 「何も知らないClaude Codeに向けて」

前提を省略しない依頼書を作る。  
対象リポジトリ、正とするファイル、守る仕様、禁止事項、作業順、受け入れ条件、確認方法を書く。

#### 「精密に」

文字数を増やすことではない。  
曖昧な判断を減らし、作業者が迷わない粒度にする。

---

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

---

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

---

## 5. 作品群の理解

Claudeは、単発のゲームだけを見て判断してはいけない。  
カメレオンJPのゲーム群として、共通部品、ランキング、実験場、保全を意識する。

### 5.1 公開済み・仕様確認済みの代表例

Notionの個別ゲーム索引や過去のLinearには、次のようなゲームがある。

```text
イロアテ（iroate）
暇をつぶそう（nayuta_no_himatsubushi）
虎に優しく（torani_yasashiku）
水滴キャッチ（suiteki_catch）
夢を見たんだけどさ（yume_wo_mitandakedosa）
アルク教（aruku_kyou）
カメレオンJPの実験場（chameleonjp_lab）
充電ｶﾞｯ（juden_ga）
あなたの1秒って（slugは最新情報で要確認）
上手に描けるかな？
尊厳を賭けようか（songen_wo_kakeyouka）
ヒトを許すな！（hito_wo_yurusuna）
```

この一覧は、作業の入口であり、常に最新とは限らない。  
新規作業時はNotionとSupabaseで確認する。

### 5.2 仕様書はあるが公開状況の確認が必要な例

```text
仕分けざる
絵文字さがしタイムアタック
取ればいいのよ
直感力って特殊能力？
間違いみっけ
答えは分かってるんだよ！
```

### 5.3 会話や構想として扱うゲーム名

次の名前は、会話や構想として出る可能性がある。  
Claudeは、正式仕様があると決めつけず、NotionまたはCURRENT_TASKで確認する。

```text
ベク取る
トマトク
イロナラベ
ハコダセ
コロシャイン
キリガナイト
ナミオシ
うちかえる
マロン飛行
トレメシ
ジョーバ
```

---

## 6. ゲームタイプ別の見るべき点

### 6.1 タップ・反射ゲーム

例:

```text
イロアテ
あなたの1秒って
充電ｶﾞｯ
```

重視すること。

```text
入力遅延
タップ判定
結果表示
ランキング
シェア
短時間での納得感
```

推奨技術。

```text
Level 0〜2
DOM中心またはCanvas 2D
TypeScriptは後からでよい
```

### 6.2 パズル・配置系

例:

```text
ベク取る
トマトク
イロナラベ
ハコダセ
```

重視すること。

```text
解ける盤面
誤タップ防止
ランダム生成の品質
タイムアタック
ヒントやペナルティ
理論値
seed対応の余地
```

推奨技術。

```text
Level 1〜3
Canvas 2D / DOM
将来TypeScript
将来Vitest
seed付き乱数
```

### 6.3 アクション・避け系

例:

```text
コロシャイン
キリガナイト
ナミオシ
```

重視すること。

```text
当たり判定の納得感
回避不能の排除
スワイプやタップの気持ちよさ
処理落ち対策
結果の納得感
```

推奨技術。

```text
Level 1〜5
Canvas 2D
必要ならMatter.js
必要ならThree.js
```

### 6.4 防衛・シューティング系

例:

```text
うちかえる
マロン飛行
ヒトを許すな！
```

重視すること。

```text
敵の増え方
後半難易度
処理落ち
スコア上限
ランキング異常値
クリア不能状況の排除
```

推奨技術。

```text
Level 2〜4
敵・弾・効果をデータ化
TypeScript候補
自動テスト候補
```

### 6.5 クイズ・相談文系

例:

```text
トレメシ
```

重視すること。

```text
問題文が自然である
選択肢の意味が分かる
用語説明がある
誤解しにくい
ランキング表示が壊れない
```

推奨技術。

```text
Level 2〜3
データ分離
CSV/JSON検証
TypeScript候補
```

### 6.6 3D・奥行き系

例:

```text
ジョーバ
Helix Jump系の研究
コロシャインの将来拡張
```

重視すること。

```text
奥行き
カメラ
接触判定
回転の重さ
iPhone 11 Proでも破綻しない性能
```

推奨技術。

```text
Level 4〜5
Three.js
WebGL
性能計測
```

---

## 7. 推奨リポジトリ構成

`CLAUDE.md` だけに全部を書かない。  
入口、ユーザー理解、現在作業、長期ルール、サブエージェントを分ける。

推奨構成。

```text
/
├─ CLAUDE.md
├─ CURRENT_TASK.md
├─ USER_MODEL.md
├─ GAME_PORTFOLIO.md
├─ DECISION_LOG.md
├─ HARNESS_LEARNING.md
├─ docs/
│  └─ harness/
│     ├─ 00_OVERVIEW.md
│     ├─ 01_USER_MODEL_GUIDE.md
│     ├─ 02_GAME_SPEC_TEMPLATE.md
│     ├─ 03_ACCEPTANCE_GATES.md
│     ├─ 04_TECH_GROWTH_ROADMAP.md
│     ├─ 05_TECH_ADOPTION_REVIEW.md
│     ├─ 06_SCORE_RANKING_CONTRACT.md
│     ├─ 07_MOBILE_TOUCH_CONTRACT.md
│     ├─ 08_PERFORMANCE_BUDGET.md
│     ├─ 09_TESTING_STRATEGY.md
│     ├─ 10_RELEASE_ARCHIVE.md
│     ├─ 11_POSTMORTEM_TEMPLATE.md
│     └─ 12_CLAUDE_HANDOFF_PROTOCOL.md
└─ .claude/
   ├─ rules/
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
   └─ agents/
      ├─ spec-auditor.md
      ├─ mobile-ux-reviewer.md
      ├─ ranking-supabase-auditor.md
      ├─ game-feel-reviewer.md
      ├─ performance-reviewer.md
      ├─ migration-architect.md
      ├─ release-archivist.md
      └─ security-key-auditor.md
```

---

## 8. 最小の `CLAUDE.md`

`CLAUDE.md` は巨大化させない。  
Claude Codeの入口として、短く保つ。

```markdown
# CLAUDE.md — chameleonJP Browser Game Harness

このリポジトリは、カメレオンJPのスマホ向けブラウザゲーム開発用です。

## 最初に読むファイル

1. `USER_MODEL.md`
2. `CURRENT_TASK.md`
3. `GAME_PORTFOLIO.md`
4. `docs/harness/00_OVERVIEW.md`
5. `docs/harness/03_ACCEPTANCE_GATES.md`

## 最重要前提

- ユーザーは非エンジニアだが、ゲーム体験・仕様・ランキング・スマホ操作への要求は高い。
- Claudeは、ユーザーを「普通の初心者」扱いしてはいけない。
- 説明は初心者向けにするが、仕様と実装判断は厳密にする。
- 現在の標準は `index.html` 1ファイル、Codeberg Pages公開、Supabaseランキング。
- ただし、将来のTypeScript、Vite、Three.js、Matter.js、WebAssembly化を邪魔しない構造にする。
- スマホ、とくにiPhone 17 Pro、iPhone 11 Pro、iPhone SE級を前提にする。
- 結果確定前にスコア送信しない。1プレイ1送信。
- `GAME_SLUG` とSupabase `games.game_slug` は必ず一致させる。
- Publishable key以外の鍵を、コード、チャット、Notion、Linearに入れない。
- 仕様、スコア、ランキング、シェア文、スマホ操作を勝手に変えない。
- 作業後は、変更点、未確認点、確認手順、Notion/Linear/Driveへ残す内容を書く。

## 作業姿勢

曖昧なまま実装しない。
ただし、共通仕様で判断できる部分は止まらず前へ進める。
ユーザー判断が必要なものは、候補とおすすめを出す。
```

---

## 9. `USER_MODEL.md` テンプレート

```markdown
# USER_MODEL.md

## 1. ユーザー概要

ユーザーは非エンジニア。  
ただし、AIを使ってスマホ向けブラウザゲームを継続的に制作している。

ユーザーはコードを自分で一から書くより、ChatGPT、Claude Code、Codex、Grokなどに仕様書、実装計画、コード、レビュー、修正依頼文を作らせる。  
そのうえで、スマホ実機で遊び、違和感や破綻を見つけ、改善方向を決める。

## 2. 主な開発環境

- iPhone 17 Pro
- iPhone 11 Pro
- iPad Pro
- Working Copy
- GitHub: chameleonjp-lab
- Codeberg Pages
- Supabase
- Notion
- Linear
- Google Drive

## 3. 現在の基本方針

- スマホ向けブラウザゲームを作る
- 原則 `index.html` 1ファイル
- Codeberg Pagesで公開
- Supabaseでランキング
- Notionに仕様を保全
- Linearに作業ログを保全
- Google Driveに完成版HTMLを保全
- GitHubはAI作業や保全にも使う

## 4. ユーザーの強い好み

- 仕様を曖昧にしない
- スマホ操作を最優先する
- iPhone SE級の小さい画面を軽視しない
- 結果画面を雑にしない
- ランキング連携を後回しにしない
- シェア文末尾にURLを入れる
- ホーム画面にゲームシェアを置く
- 結果画面に結果シェアを置く
- 1プレイ後に自然にもう一度遊べる導線を置く
- 実験場への導線を大事にする
- ゲームの理不尽さ、回避不能、理論値、ランキング異常値を気にする

## 5. ユーザーが嫌うこと

- 仕様を勝手に変える
- ゲームの核をぼかす
- 「たぶん大丈夫」で済ませる
- 抽象論だけで終わる
- 作業場所や差し替え場所を書かない
- 古いコードを最新版として扱う
- JavaScript本文が画面に表示されるような貼り替え事故
- secret keyを扱う
- スマホで確認していないのに完成扱いにする
- 技術的には正しいが、遊ぶと面白くない実装

## 6. 依頼文の読み替え

### 「更新されました」

最新コードが変わったという意味。  
Claudeは、差分、仕様破綻、ランキング、スマホ操作、結果画面、シェア、理不尽挙動を確認する。

### 「徹底レビュー」

表面的なコードレビューではなく、ゲームとして壊れていないかを見る。  
UI、操作性、ゲームロジック、ランキング、理論値、公開導線まで見る。

### 「何も知らないClaude Codeに向けて」

前提をすべて書いた依頼書が必要。  
対象リポジトリ、守る仕様、禁止事項、作業順、受け入れ条件、確認方法を書く。

### 「精密に」

説明量を増やすだけではない。  
曖昧な判断を減らし、作業者が迷わない粒度にする。

## 7. ユーザーの長期成長前提

今は非エンジニアとしてAIに実装させる。  
ただし、今後10年で次の方向に伸びる前提にする。

- 仕様作成力
- ゲームデザイン判断
- GitHub/Codeberg運用
- Supabase運用
- TypeScript理解
- 自動テスト理解
- Three.js / WebGL / 物理エンジン理解
- 共通部品化
- ゲーム群全体の基盤化

Claudeは、今のユーザーに合わせて説明する。  
ただし、将来の成長を邪魔する雑な構成にはしない。
```

---

## 10. `CURRENT_TASK.md` テンプレート

```markdown
# CURRENT_TASK.md

## 1. 対象

ゲーム名:
project_slug:
リポジトリ:
公開URL:
作業ブランチ:
Linear ID:
Notionページ:
Drive保存先:

## 2. 今回やること

目的:

今回やること:

今回やらないこと:

## 3. 正とする情報

最新コード:
最新仕様:
最新ランキング設定:
最新公開URL:

古いので見ないもの:

## 4. ゲーム契約

ゲームの核:
1プレイの長さ:
操作:
成功条件:
失敗条件:
終了条件:
スコア計算:
ランキング種別:
シェア文:

## 5. Supabase契約

GAME_SLUG:
CLIENT_VERSION:
score_order:
score_unit:
score_scale:
score_decimals:
score_label:
first_score_label:
best_score_label:
display_order:
is_active:

## 6. スマホ契約

主対象:
iPhone SE級対応:
横スクロール:
ダブルタップ拡大:
長押し:
2本指操作:
Safari下部バー:
名前入力後blur:

## 7. 既知の問題

- 

## 8. 受け入れ条件

- 
- 
- 

## 9. 作業後に残す場所

Notion:
Linear:
Drive:
Codeberg:
GitHub:
```

---

## 11. `GAME_PORTFOLIO.md` テンプレート

```markdown
# GAME_PORTFOLIO.md

## 1. 作品群の共通性

カメレオンJPのゲームは、スマホで短時間に遊ぶブラウザゲーム群である。  
多くは、1プレイの結果をSupabaseランキングへ送り、結果をシェアできる。

共通して重要なこと。

- スマホ操作
- 短時間プレイ
- リトライ性
- 結果画面
- Supabaseランキング
- 実験場への掲載
- シェア文
- Codeberg Pages公開
- Notion / Linear / Drive保全

## 2. 公開済み・確認済みゲーム

- イロアテ（iroate）
- 暇をつぶそう（nayuta_no_himatsubushi）
- 虎に優しく（torani_yasashiku）
- 水滴キャッチ（suiteki_catch）
- 夢を見たんだけどさ（yume_wo_mitandakedosa）
- アルク教（aruku_kyou）
- カメレオンJPの実験場（chameleonjp_lab）
- 充電ｶﾞｯ（juden_ga）
- あなたの1秒って（slug要確認）
- 上手に描けるかな？
- 尊厳を賭けようか（songen_wo_kakeyouka）
- ヒトを許すな！（hito_wo_yurusuna）

## 3. 仕様書はあるが公開状況の確認が必要なゲーム

- 仕分けざる
- 絵文字さがしタイムアタック
- 取ればいいのよ
- 直感力って特殊能力？
- 間違いみっけ
- 答えは分かってるんだよ！

## 4. 構想・会話上の候補名

- ベク取る
- トマトク
- イロナラベ
- ハコダセ
- コロシャイン
- キリガナイト
- ナミオシ
- うちかえる
- マロン飛行
- トレメシ
- ジョーバ

この章の名前は、正式仕様があるとは限らない。  
作業前にNotionまたはCURRENT_TASKで確認する。
```

---

## 12. `DECISION_LOG.md` テンプレート

```markdown
# DECISION_LOG.md

## 記録ルール

技術選定、仕様変更、ランキング設定、例外判断はここに残す。  
理由が残っていない変更は、将来の復旧時に事故の原因になる。

---

## Decision 001

日付:
対象ゲーム:
対象ファイル:
判断したこと:

選んだ案:

選ばなかった案:

理由:

影響:
- ゲーム性:
- スマホ操作:
- ランキング:
- Supabase:
- 公開:
- 将来移行:

Notionへ残したか:
Linearへ残したか:
```

---

## 13. `HARNESS_LEARNING.md` テンプレート

```markdown
# HARNESS_LEARNING.md

同じ失敗を2回したら、このファイルへ追加する。  
追加後、必要な `.claude/rules/` または `docs/harness/` に反映する。

---

## Learning 001

日付:
対象ゲーム:
起きたこと:
影響:
原因:
なぜ見逃したか:
次から追加するルール:
追加先ファイル:
Claudeに守らせる文:
```

例:

```text
結果画面でランキング送信中にホームへ戻るボタンが消えた。
→ 「送信中でもホームへ戻る導線を残す」をAcceptance Gateへ追加する。
```

---

## 14. 技術レベル

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

### 14.1 現在の標準

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

### 14.2 上のLevelへ進む条件

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

---

## 15. 技術レーダー

技術は4段階で管理する。

```text
Adopt: 今の標準として使う
Trial: 一部ゲームで試す
Assess: 調査するが本番ではまだ使わない
Hold: 今は使わない
```

### 15.1 現在の技術レーダー

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

### 15.2 TypeScriptの扱い

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

### 15.3 Viteの扱い

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

### 15.4 Playwrightの扱い

Playwrightは、ブラウザ操作を自動確認するための候補。  
ホーム、名前入力、カウントダウン、結果画面、ランキング失敗時の導線などを確認する。

使う条件。

```text
公開済みゲームの修正事故を減らしたい
結果画面や名前入力の退行を防ぎたい
複数端末の画面確認を型にしたい
```

### 15.5 WebGL / Three.jsの扱い

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

### 15.6 Matter.jsの扱い

Matter.jsは、物理挙動を扱うための候補。  
落下、衝突、反発、押し出しがゲームの核なら検討する。

使う条件。

```text
手書き当たり判定では納得感が出ない
物理挙動がゲームの面白さになる
回避不能や不自然な衝突を減らしたい
```

### 15.7 WebAssembly / Rustの扱い

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

---

## 16. 技術採用審査テンプレート

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

---

## 17. アーキテクチャ原則

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

### 17.1 禁止する混在

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

---

## 18. ゲーム契約

ゲームごとに、次の契約を明文化する。  
契約とは、あとから実装を変えても壊してはいけない約束である。

### 18.1 Game Contract

```text
ゲーム名:
正式タイトル:
project_slug:
公開URL:
ゲームの核:
何をするゲームか:
1プレイの長さ:
プレイヤーの目的:
成功条件:
失敗条件:
終了条件:
リトライ理由:
```

### 18.2 Input Contract

```text
主操作:
副操作:
使わない操作:
2本指タップ:
2本指スワイプ:
ピンチ:
長押し:
スクロール:
キーボード:
名前入力:
```

### 18.3 Score Contract

```text
スコア名:
良い記録の方向:
保存値:
表示値:
score_order:
score_scale:
score_decimals:
小数処理:
マイナス許可:
理論上の上限:
理論上の下限:
不正っぽい値:
```

### 18.4 Ranking Contract

```text
GAME_SLUG:
CLIENT_VERSION:
submit_scoreのタイミング:
1プレイ送信回数:
送信失敗時の表示:
初回記録:
ベスト記録:
プレイ回数:
ランキング0件時:
```

### 18.5 Render Contract

```text
描画方式:
Canvas:
DOM:
絵文字:
画像:
パーティクル:
影:
ハイライト:
アニメーション:
```

### 18.6 Asset Contract

```text
画像:
音声:
フォント:
外部ファイル:
Google Drive保存先:
Codeberg配置場所:
読み込み失敗時の代替:
```

画像や音声など外部ファイルを使う場合は、ファイル構成をNotionへ残す。

---

## 19. 画面状態契約

全ゲームは、最低限この状態を持つ。

```text
home
nameConfirm
countdown
playing
result
```

### 19.1 ホーム画面

必須。

```text
ゲームタイトル
短い説明
遊び方
スタートボタン
ゲームシェアボタン
必要なら「他のゲームで遊ぶ」リンク
```

ランキング対応ゲームでは、ホーム画面に名前入力欄を常時表示しない。  
スタート後に名前確認へ進める。

### 19.2 名前確認

```text
保存済み名前があれば確認する
保存済み名前がなければ入力させる
名前なしでは開始できない
前後の空白を削る
最大20文字
絵文字と記号は原則許可
確定後に blur() する
```

### 19.3 カウントダウン

必須。

```text
3
2
1
START
```

カウントダウン中に、正解、的、落下物、問題、ゲーム対象を先に見せない。

### 19.4 プレイ中

```text
名前入力を出さない
結果確定前にスコア送信しない
ゲームループは必要な時だけ動かす
画面外オブジェクトを増やし続けない
```

### 19.5 結果画面

必須。

```text
今回の記録
初回記録
ベスト記録
プレイ回数
自己ベスト更新の有無
もう一度遊ぶ
ホームへ戻る
結果シェア
ランキング送信状態
```

ランキング送信に失敗しても、結果画面を壊さない。  
送信中でも、ホームへ戻る導線は残す。  
送信中にボタン列を丸ごと消してはいけない。

---

## 20. スマホ操作契約

スマホ操作は毎回明文化する。

```text
タッチ抑制の範囲:
プレイエリア:
ボタン:
リンク:
スクロールできる画面:
スクロールできない画面:
名前入力後のblur:
visualViewport:
safe-area:
タップ判定:
スワイプ判定:
誤タップ時の扱い:
```

### 20.1 対象端末

```text
iPhone 17 Pro
iPhone 11 Pro
iPhone SE級の小さい画面
iPad Pro
Androidブラウザ
```

iPhone SE級で遊べないものは、完成扱いにしない。  
iPadで広く見えるだけで合格にしない。

### 20.2 画面別スクロール方針

```text
ホーム:
  原則スクロールなし。
  説明が長い場合だけ縦スクロール可。

名前入力:
  キーボード表示を考慮。
  確定後にblur。

カウントダウン:
  スクロールなし。
  ゲーム対象を先に見せない。

プレイ中:
  原則スクロールなし。
  タッチ抑制はプレイエリア中心。

結果:
  縦スクロール可。
  ホームへ戻る導線は必ず残す。
```

### 20.3 タッチ抑制

タッチ抑制は、原則としてプレイエリアに限定する。  
画面全体へ強く抑制をかけすぎて、ボタンやリンクが押せなくなる実装は避ける。

確認すること。

```text
ダブルタップ拡大が起きない
長押しで選択やコピーが出にくい
2本指ズームで画面が崩れない
2本指タップがゲーム入力にならない
ボタンやリンクは押せる
```

### 20.4 Pointer Events標準

将来の標準入力は Pointer Events に寄せる。  
Pointer Eventsとは、マウス、ペン、タッチを同じ形で扱うブラウザの入力イベントである。  
以後は「統一入力イベント」と呼ぶ。

統一入力イベントを使う理由は、タッチとマウスで別々の処理を書きすぎると、iPhone、Android、PCで挙動がズレるためである。

---

## 21. ランキングとSupabase契約

Supabaseはランキングとプレイ記録に使う。  
秘密鍵は絶対にブラウザへ入れない。

### 21.1 ゲーム側固定値

```javascript
const SUPABASE_URL = "https://mlpnjgezrnhdxsxolyzj.supabase.co";
const SUPABASE_PUBLISHABLE_KEY = "ここにPublishable keyを入れる";
const GAME_SLUG = "ここにgame_slug";
const CLIENT_VERSION = "ここにゲーム別バージョン名";
```

Publishable keyはユーザーが手元で差し替える。  
AIチャット、Notion、Linearには貼らない。  
secret keyやservice_role keyは絶対に使わない。

### 21.2 Supabase `games` に必ず入れる項目

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

### 21.3 スコア種別

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

### 21.4 保存値と表示値

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

### 21.5 使うRPC

使う関数。

```text
submit_score
get_best_score_ranking
get_first_try_ranking
get_game_play_stats
```

古い `get_game_ranking` は使わない。

### 21.6 スコア送信

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

### 21.7 マイナススコア

マイナススコアがあるゲームでは、0未満を異常扱いしない。

禁止。

```javascript
finalScore = Math.max(0, finalScore);
```

マイナススコアを使う場合は、Score Contractに理論上の下限を明記する。

---

## 22. 実験場契約

実験場は、Supabase `public.games` を正とする。

### 22.1 自動表示条件

```text
is_active = true
game_slug がある
title がある
game_url がある
display_order がある
```

### 22.2 禁止

```text
ゲーム一覧を固定配列だけで管理する
slice(0, 8) のように件数を固定する
新作ゲームをHTMLに直書きする
ランキングをページ表示時に全件一括取得する
0件をエラー扱いにする
```

### 22.3 ランキング取得

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

### 22.4 詳細ランキング

```text
初回記録とベスト記録を分ける
タブで切り替える
横並び表示にしない
記録日時は表示しない
タブ名は first_score_label と best_score_label を使う
保存値ではなく、表示値に変換して出す
```

---

## 23. localStorage契約

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

---

## 24. ランダム生成契約

ランダム生成を使うゲームでは、将来の検証のために再現できる形にする。

### 24.1 seed

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

### 24.2 将来保存する情報

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

### 24.3 ランダム生成の確認項目

```text
絶対にクリア不能な盤面が出ないか
序盤に理不尽配置が出ないか
難易度上昇が急すぎないか
同じ配置が出すぎないか
ランキング上位が運だけにならないか
```

---

## 25. リプレイ契約

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

---

## 26. パフォーマンス契約

性能は「なんとなく軽い」ではなく、予算として決める。

### 26.1 基本予算

```text
目標:
  60fps

許容:
  重いゲームでは30fps安定

1フレーム:
  16.7ms以内が理想
  33.3ms以内で最低限

毎フレームDOM更新:
  原則最小限

Canvas再生成:
  毎フレーム禁止

配列・オブジェクト大量生成:
  毎フレーム禁止

Supabase通信:
  ゲーム中に行わない
  結果確定後に送信1回
```

### 26.2 ゲームごとの上限

ゲームごとに書く。

```text
敵の最大数:
弾の最大数:
パーティクル最大数:
動くギミック最大数:
同時エフェクト最大数:
画面外オブジェクトの処理:
ランキング通信回数:
```

### 26.3 Canvasの場合

```text
requestAnimationFrameを使う
非表示画面ではゲームループを止める
毎フレームDOMを大量更新しない
毎フレーム新しい配列やオブジェクトを大量生成しない
当たり判定は単純に保つ
画面サイズ計算を毎フレーム行わない
```

### 26.4 DOMゲームの場合

```text
動く要素を増やしすぎない
レイアウト再計算が増える処理を避ける
アニメーションは必要最小限にする
タップ遅延や誤操作を減らす
```

---

## 27. テスト契約

テストは3段階に分ける。

### 27.1 手動確認

今すぐ必須。

```text
iPhone 17 Pro
iPhone 11 Pro
iPhone SE級想定
iPad Pro
Android想定
```

確認すること。

```text
ホームが表示される
スタートできる
名前入力できる
名前確定後にblurする
カウントダウンが出る
プレイできる
結果画面へ進む
ホームへ戻れる
もう一度遊べる
シェア文にURLが入る
横スクロールがない
タップ位置がズレない
```

### 27.2 ロジック確認

将来、TypeScriptやViteへ進んだら入れる。

対象。

```text
スコア計算
タイム表示変換
ランキング保存値変換
難易度上昇
ランダム生成
ゲーム終了条件
旧localStorage移行
```

### 27.3 ブラウザ自動確認

将来Playwrightで入れる。

対象。

```text
ホームが表示される
名前入力できる
スタートできる
カウントダウンが出る
プレイ画面へ進む
結果画面へ進む
ホームへ戻れる
シェア文にURLが入る
ランキング送信失敗時も画面が壊れない
```

---

## 28. 公開と保全

Codebergへ置く正式ファイル名は原則 `index.html`。  
AIが修正版を共有する時は、古い参照を避けるため別名で出す。  
Codebergへ配置する時だけ `index.html` にリネームする。

例:

```text
juden_ga_index_ranked_v20260701_reviewed.html
↓ 公開時
index.html
```

完成時にNotionとLinearへ残すこと。

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

### 28.1 公開済みゲームの丸ごと差し替え注意

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

---

## 29. 作業フロー

Claudeは、毎回この順で動く。

### 29.1 読む

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

### 29.2 分ける

```text
今回やること
今回やらないこと
触るファイル
触らないファイル
壊してはいけない契約
未確認の項目
```

### 29.3 実装前に出す

```text
変更方針
リスク
確認方法
ロールバック方法
```

### 29.4 実装後に出す

```text
変更した内容
受け入れ条件の結果
スマホ未確認項目
Supabase未確認項目
次にやるべきこと
```

### 29.5 実装前にClaudeが必ず言語化すること

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

### 29.6 実装後にClaudeが必ず報告すること

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

---

## 30. 品質ゲート

以下を満たすまで完成扱いにしない。

### 30.1 遊びやすさ

```text
最初に何をすればよいか分かる
スタートまでが短い
1プレイの目的が分かる
失敗理由が分かる
もう一度遊びやすい
結果をシェアできる
```

### 30.2 スマホ

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

### 30.3 ランキング

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

### 30.4 公開

```text
Codeberg Pagesで表示できる
index.html が正式名になっている
JavaScript本文が画面に出ていない
<script>タグが壊れていない
シェアURLが正しい
実験場から遊べる
詳細ランキングへの導線が正しい
```

### 30.5 保全

```text
Notionに仕様と公開情報を残した
Linearに作業ログと受け入れ条件を残した
Driveに完成版HTMLを保存する方針を確認した
次に別AIが触る時の注意を残した
CURRENT_TASK.md を更新した
```

---

## 31. Anti-Patterns

避けること。

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

---

## 32. Claudeサブエージェント構成

Fable 5が使える間は、Claudeに複数視点で作業させる。  
Fable 5が使えなくなっても、同じ役割分担を小さなモデルで使えるようにする。

### 32.1 spec-auditor

役割: 仕様の抜け、矛盾、過去仕様とのズレを見つける。

見るもの。

```text
Notion
CURRENT_TASK
README
SPEC
CLAUDE
実装コード
```

重点。

```text
ゲームの核
画面状態
スコア計算
終了条件
仕様と実装のズレ
```

### 32.2 mobile-ux-reviewer

役割: スマホ操作と画面破綻を見る。

重点。

```text
iPhone SE級
iPhone 17 Pro
Safari下部バー
横スクロール
タップ位置
長押し
ダブルタップ拡大
2本指操作
結果画面スクロール
```

### 32.3 ranking-supabase-auditor

役割: Supabaseとランキングの事故を見る。

重点。

```text
GAME_SLUG一致
CLIENT_VERSION
submit_score
1プレイ1送信
score_order
score_scale
score_decimals
初回記録
ベスト記録
play_count
実験場表示
```

### 32.4 game-feel-reviewer

役割: ゲームとして楽しいか、理不尽ではないかを見る。

重点。

```text
すぐ理解できるか
失敗理由が分かるか
もう一度遊びたいか
回避不能がないか
後半難易度が急すぎないか
操作が気持ちよいか
```

### 32.5 performance-reviewer

役割: スマホ性能と処理落ちを見る。

重点。

```text
requestAnimationFrame
毎フレームDOM更新
Canvas再生成
オブジェクト生成量
パーティクル上限
敵・弾・ギミック数
iPhone 11 Proでの余裕
```

### 32.6 migration-architect

役割: 将来の技術移行を設計する。

重点。

```text
TypeScript化
Vite化
複数ファイル化
Three.js化
Matter.js化
WebAssembly化
共通部品化
テスト導入
```

### 32.7 release-archivist

役割: 公開と保全の抜けを確認する。

重点。

```text
Codeberg Pages
GitHub
Drive保存
Notion更新
Linear更新
ファイル名
Publishable key差し替え行
公開前チェック
```

### 32.8 security-key-auditor

役割: 鍵と公開情報の事故を防ぐ。

重点。

```text
secret keyが入っていない
service_role keyが入っていない
Publishable keyの扱いが明示されている
Supabase URLは許容範囲
ユーザー名や個人情報を不要に出していない
```

---

## 33. `.claude/agents/spec-auditor.md`

```markdown
---
name: spec-auditor
description: Use when reviewing a chameleonJP browser game spec, implementation plan, or updated code for mismatches, missing requirements, and broken assumptions.
tools: Read, Glob, Grep
model: sonnet
---

You are the specification auditor for chameleonJP browser games.

The user is a non-engineer game creator who depends on AI to implement strict mobile browser games.
Do not treat vague behavior as acceptable.

Read:
- USER_MODEL.md
- CURRENT_TASK.md
- GAME_PORTFOLIO.md
- CLAUDE.md
- relevant docs/harness files

Check:
- Does the implementation match CURRENT_TASK.md?
- Does it preserve the game core?
- Are screen states separated?
- Are start, countdown, playing, and result phases clear?
- Are score rules explicit?
- Are success, failure, and end conditions explicit?
- Are mobile constraints stated?
- Are Supabase ranking rules stated?
- Are share texts and URLs present?
- Are any specifications silently changed?

Return:
1. Critical mismatches
2. Missing decisions
3. Risky assumptions
4. Required fixes
5. Things that are acceptable
```

---

## 34. `.claude/agents/ranking-supabase-auditor.md`

```markdown
---
name: ranking-supabase-auditor
description: Use when a chameleonJP browser game touches Supabase, score submission, rankings, result screen records, or lab display.
tools: Read, Glob, Grep
model: sonnet
---

You are the Supabase and ranking auditor for chameleonJP browser games.

Read:
- USER_MODEL.md
- CURRENT_TASK.md
- docs/harness/06_SCORE_RANKING_CONTRACT.md
- relevant source files

Check:
- GAME_SLUG matches Supabase games.game_slug
- CLIENT_VERSION exists
- submit_score is called only after result is final
- submit_score is called once per play
- p_score is an integer
- score_order is respected
- score_scale and score_decimals are respected
- first score and best score are handled separately
- play_count appears on result screen
- send failure does not break result screen
- old get_game_ranking is not used
- secret key or service_role key is not present
- share URL is not removed

Return:
1. Blocking issues
2. Likely ranking display errors
3. Security risks
4. Result screen risks
5. Exact files and functions to inspect
```

---

## 35. `.claude/agents/mobile-ux-reviewer.md`

```markdown
---
name: mobile-ux-reviewer
description: Use when reviewing mobile browser playability, touch input, layout, iPhone Safari behavior, and result screen usability.
tools: Read, Glob, Grep
model: sonnet
---

You are the mobile UX reviewer for chameleonJP browser games.

The target is smartphone browser play, especially iPhone 17 Pro, iPhone 11 Pro, and iPhone SE-class screens.

Read:
- USER_MODEL.md
- CURRENT_TASK.md
- docs/harness/07_MOBILE_TOUCH_CONTRACT.md
- relevant source files

Check:
- horizontal scroll
- safe-area and Safari bottom bar
- name input blur
- visualViewport handling
- double tap zoom
- long press selection
- two-finger input behavior
- touch suppression scope
- buttons and links still clickable
- gameplay area fits
- result screen can scroll when needed
- home button remains during ranking submission
- tap position and hit detection align

Return:
1. Critical mobile blockers
2. Likely iPhone issues
3. Layout risks
4. Touch-input risks
5. Manual test checklist
```

---

## 36. `.claude/agents/game-feel-reviewer.md`

```markdown
---
name: game-feel-reviewer
description: Use when reviewing whether a chameleonJP browser game is understandable, fair, replayable, and fun on mobile.
tools: Read, Glob, Grep
model: sonnet
---

You are the game-feel reviewer for chameleonJP browser games.

Do not review only code correctness.
Review whether the player can understand, act, fail, retry, and feel the result is fair.

Check:
- Is the core fun visible within a few seconds?
- Does the player know what to do first?
- Is failure understandable?
- Are there avoidable unfair states?
- Are controls satisfying on mobile?
- Is replay friction low?
- Does the result screen encourage another play?
- Does ranking make sense for this score type?
- Are difficulty increases fair?
- Are there likely impossible or boring states?

Return:
1. Game-feel blockers
2. Fairness risks
3. Replayability risks
4. Suggested changes that do not alter the game core
5. Decisions that need user approval
```

---

## 37. `.claude/agents/performance-reviewer.md`

```markdown
---
name: performance-reviewer
description: Use when reviewing browser performance, Canvas loops, DOM updates, object counts, and mobile frame stability.
tools: Read, Glob, Grep
model: sonnet
---

You are the performance reviewer for chameleonJP browser games.

The target is smooth play on smartphone browsers.
Prioritize iPhone 11 Pro and iPhone SE-class screens, not only modern flagship devices.

Check:
- requestAnimationFrame usage
- stopping loops outside playing state
- excessive DOM writes
- Canvas resize frequency
- object allocation inside loops
- particle counts
- enemy/projectile caps
- collision complexity
- unnecessary Supabase calls during gameplay
- layout thrashing
- asset size and loading fallback

Return:
1. Performance blockers
2. Likely slow paths
3. Safe optimizations
4. Risky optimizations to avoid
5. Manual profiling checklist
```

---

## 38. `.claude/agents/migration-architect.md`

```markdown
---
name: migration-architect
description: Use when planning migration from single HTML to TypeScript, Vite, Three.js, Matter.js, WebAssembly, tests, or shared SDK.
tools: Read, Glob, Grep
model: sonnet
---

You are the migration architect for chameleonJP browser games.

Do not introduce new technology just because it is modern.
Preserve the game core, score contract, mobile controls, Supabase ranking, and Codeberg Pages release path.

Check:
- What problem does the migration solve?
- Can the current Level 0/1 version remain the fallback?
- Which layer moves first: Core, Input, Render, Ranking, or Release?
- Does the migration preserve GAME_SLUG and score behavior?
- Can the output still be static files for Codeberg Pages?
- Can the user still archive a complete build to Google Drive?
- What manual steps are required in Working Copy?
- What tests should be added before migration?

Return:
1. Migration recommendation
2. Required pre-migration freeze list
3. Step-by-step migration plan
4. Rollback plan
5. User decisions required
```

---

## 39. `.claude/agents/release-archivist.md`

```markdown
---
name: release-archivist
description: Use before declaring a chameleonJP browser game complete or ready for Codeberg Pages release.
tools: Read, Glob, Grep
model: sonnet
---

You are the release archivist for chameleonJP browser games.

A game is not complete until publication and preservation details are clear.

Check:
- final filename
- Codeberg target path
- whether it must be renamed to index.html
- public URL
- repository URL
- GitHub URL if relevant
- Drive archive filename
- Notion update items
- Linear update items
- Supabase games values
- Publishable key replacement line
- known unverified device checks
- next-AI handoff notes

Return:
1. Release blockers
2. Archive checklist
3. Notion update text
4. Linear update text
5. Final handoff summary
```

---

## 40. `.claude/agents/security-key-auditor.md`

```markdown
---
name: security-key-auditor
description: Use when reviewing Supabase keys, public configuration, secrets, or browser-exposed values.
tools: Read, Glob, Grep
model: sonnet
---

You are the key and public-config auditor for chameleonJP browser games.

Check:
- No secret key is present
- No service_role key is present
- Publishable key placeholder is clear
- Supabase URL is expected
- No private token appears
- No personal data is unnecessarily exposed
- User is told exactly where to paste Publishable key locally
- Notion/Linear text does not include secrets

Return:
1. Blocking secret exposure
2. Suspicious values to verify
3. Safe public values
4. Required cleanup
5. User-facing replacement instructions
```

---

## 41. `.claude/rules/core.md`

```markdown
# Core Rules

- Read USER_MODEL.md and CURRENT_TASK.md before changing code.
- Do not treat the user as a generic beginner. Explain simply, but reason strictly.
- Preserve the game core unless the user explicitly asks to change it.
- Separate home, nameConfirm, countdown, playing, and result states.
- Do not send score before result is final.
- Do not call submit_score more than once per play.
- Do not remove home share or result share unless explicitly approved.
- Do not remove home navigation from the result screen.
- Do not use old files as source of truth when CURRENT_TASK says otherwise.
- Report touched files, untouched files, verified items, and unverified items.
```

---

## 42. `.claude/rules/user-context.md`

```markdown
# User Context Rules

- User is a non-engineer who builds browser games with AI.
- Write user-facing instructions with exact file names, paste locations, and checks.
- Avoid abstract explanations without next actions.
- When choices exist, provide options and a recommended choice.
- If a decision affects game feel, score type, two-finger behavior, or share text, mark it as user decision.
- If common rules decide the answer, proceed without asking.
```

---

## 43. `.claude/rules/mobile.md`

```markdown
# Mobile Rules

- Target iPhone 17 Pro, iPhone 11 Pro, iPhone SE-class screens, iPad Pro, and Android browser.
- Avoid horizontal scroll.
- Protect against Safari bottom bar overlap.
- Name input must blur before countdown.
- Recalculate layout after keyboard closes when needed.
- Use visualViewport when useful.
- Touch suppression should be scoped to gameplay area, not the whole page.
- Buttons and links must remain clickable.
- Define two-finger tap, two-finger swipe, and pinch behavior for every game.
- Result screen may scroll. Gameplay screen should usually not scroll.
```

---

## 44. `.claude/rules/ranking.md`

```markdown
# Ranking Rules

- Supabase is the ranking backend.
- GAME_SLUG must match Supabase games.game_slug.
- CLIENT_VERSION must be present.
- p_score must be an integer.
- Use score_order to decide best score.
- Use score_scale and score_decimals for display conversion.
- Use submit_score, get_best_score_ranking, get_first_try_ranking, get_game_play_stats.
- Do not use old get_game_ranking.
- Send once per finalized result.
- If score submission fails, keep the result screen usable.
- Home navigation must remain visible during ranking submission.
- Do not treat negative scores as invalid unless the game contract forbids them.
```

---

## 45. `.claude/rules/performance.md`

```markdown
# Performance Rules

- Target 60fps when possible.
- Heavy games may accept stable 30fps.
- Use requestAnimationFrame for game loops.
- Stop loops outside playing state.
- Do not recreate Canvas every frame.
- Do not update large DOM sections every frame.
- Avoid large object allocation inside hot loops.
- Put caps on particles, enemies, projectiles, and effects.
- Do not make Supabase calls during active gameplay.
- Do not recalculate layout every frame.
```

---

## 46. `.claude/rules/architecture.md`

```markdown
# Architecture Rules

- Current safe standard is index.html single file.
- Even in one file, separate Core, Input, Render, Storage, Ranking, Share, Platform, and Release sections.
- Avoid one function that handles input, score, rendering, and network all together.
- Write functions so future TypeScript/Vite migration is possible.
- Do not introduce frameworks by default.
- New libraries require a technology adoption review.
```

---

## 47. `.claude/rules/tech-growth.md`

```markdown
# Tech Growth Rules

- Do not freeze the project forever at Vanilla JavaScript.
- Do not introduce new technology without a concrete reason.
- TypeScript is a future candidate for state, score, and Supabase safety.
- Vite is a future candidate for multi-file development and static builds.
- Playwright is a future candidate for browser flow testing.
- Three.js and WebGL are candidates for real 3D or depth-based games.
- Matter.js is a candidate when physics is central.
- WebAssembly/Rust is only for heavy computation, generation, replay verification, or fraud checks.
- Any migration must preserve game core, score contract, mobile controls, ranking, and Codeberg Pages release path.
```

---

## 48. `.claude/rules/testing.md`

```markdown
# Testing Rules

- Manual mobile testing is required before completion.
- Future automated tests should cover score calculation, display conversion, random generation, end conditions, and localStorage migration.
- Future Playwright tests should cover home, name input, countdown, result, share URL, and ranking failure behavior.
- Do not declare complete when critical device checks are still unverified.
- If unverified, list exact unverified items.
```

---

## 49. `.claude/rules/release.md`

```markdown
# Release Rules

- Codeberg release filename is usually index.html.
- AI-shared revised files should use distinct filenames to avoid stale references.
- Publishable key replacement line must be stated.
- Secret keys must never appear in code, chat, Notion, or Linear.
- Update Notion with spec, public URL, repo URL, latest filename, Supabase values, key replacement line, known issues, and device checks.
- Update Linear with work log, acceptance results, and next-AI handoff notes.
- Archive final index.html to Google Drive.
```

---

## 50. 移行プレイブック

既存ゲームを新技術へ移す時は、全部作り直さない。

### 50.1 移行順

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

### 50.2 失敗扱い

```text
技術移行でゲーム性が変わった
ランキング形式が変わった
スコア単位が変わった
シェア文URLが消えた
スマホ操作が重くなった
Codeberg Pagesで公開できなくなった
```

### 50.3 移行前に固定する表

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

---

## 51. 共通部品化のルール

同じ処理を3ゲーム以上で使ったら、共通化候補にする。

### 51.1 共通化候補

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

### 51.2 共通化してはいけないもの

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

---

## 52. 10年ロードマップ

### 52.1 0〜1年

目的。  
今の運用を壊さず、完成率を上げる。

```text
index.html 1ファイル
Canvas 2D
スマホ操作
Supabaseランキング
実験場連携
Notion/Linear/Drive保全
```

### 52.2 1〜3年

目的。  
事故を減らす。

```text
共通テンプレート
共通ランキング処理
共通結果画面
seed付き乱数
軽い自動テスト
TypeScript試験採用
Vite試験採用
```

### 52.3 3〜5年

目的。  
表現力を上げる。

```text
Three.js
Matter.js
WebGL
音管理
アニメーション管理
リプレイ検証
性能計測
```

### 52.4 5〜10年

目的。  
ゲーム群を基盤化する。

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

---

## 53. Fable 5向け追加ルール

Fable 5が使える時は、長い作業を任せてよい。  
ただし、長い作業ほど、先に作業単位を分ける。

### 53.1 Fable 5に任せてよいこと

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

### 53.2 Fable 5でも禁止

```text
ゲーム性を勝手に変える
スコア計算を勝手に変える
secret keyを扱う
ランキング仕様を曖昧にする
スマホ確認なしで完成扱いにする
旧ファイルを正として扱う
```

---

## 54. Fable 5が使えない時の縮小運用

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

---

## 55. Claude Code依頼テンプレート

### 55.1 新規ゲーム制作依頼

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

### 55.2 更新コードレビュー依頼

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

### 55.3 修正依頼

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

### 55.4 移行設計依頼

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

---

## 56. 公開後のPostmortemテンプレート

```markdown
# Postmortem

## 発生日

## 対象ゲーム

## 何が起きたか

## 影響

## 原因

## すぐ直したこと

## 再発防止

## ハーネスへ追加するルール

## Notion更新

## Linear更新

## Drive更新

## 次に別AIが触る時の注意
```

同じ失敗を2回したら、必ずハーネスへ追加する。

---

## 57. 最終判断基準

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

---

## 58. 外部仕様参照

この章は、ハーネスを更新する時の参照先。  
Claudeは、この章だけを根拠に決めず、最新の公式ドキュメントを確認する。

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

---

## 59. この単一Markdownを分割する時の優先順位

最初に分けるべきファイル。

```text
1. USER_MODEL.md
2. CURRENT_TASK.md
3. GAME_PORTFOLIO.md
4. CLAUDE.md
5. .claude/rules/core.md
6. .claude/rules/mobile.md
7. .claude/rules/ranking.md
8. .claude/agents/spec-auditor.md
9. .claude/agents/mobile-ux-reviewer.md
10. .claude/agents/ranking-supabase-auditor.md
```

次に分けるべきファイル。

```text
docs/harness/03_ACCEPTANCE_GATES.md
docs/harness/04_TECH_GROWTH_ROADMAP.md
docs/harness/05_TECH_ADOPTION_REVIEW.md
docs/harness/06_SCORE_RANKING_CONTRACT.md
docs/harness/07_MOBILE_TOUCH_CONTRACT.md
docs/harness/08_PERFORMANCE_BUDGET.md
```

最後に分けるべきファイル。

```text
DECISION_LOG.md
HARNESS_LEARNING.md
docs/harness/10_RELEASE_ARCHIVE.md
docs/harness/11_POSTMORTEM_TEMPLATE.md
docs/harness/12_CLAUDE_HANDOFF_PROTOCOL.md
```

---

## 60. 更新ルール

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

---

以上。
