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
