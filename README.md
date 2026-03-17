# /create-lp

Claude Codeに「LP作って」と言うだけで、ヒアリングからデザイン、コーディング、デプロイまで全部やってくれるスキルです。

---

## これは何？

[Claude Code](https://docs.anthropic.com/en/docs/claude-code)（AnthropicのCLIツール）に追加できる**カスタムスキル**です。

インストールすると `/create-lp` というスラッシュコマンドが使えるようになります。実行するとClaudeが対話形式でLPの要件を聞いてきて、構成を提案し、コードを書き、ブラウザで確認しながら修正し、最終的にGitHub Pagesで公開するところまで一気にやってくれます。

## 何が出てくるか

**1ファイル完結のHTML**が出てきます。CSS・JavaScriptもすべてHTMLの中に入っているので、そのファイルをブラウザで開けばそのまま動きます。

こんな感じのLPが作れます:
- ヒーローセクション（キャッチコピー + プロダクトモック）
- 課題提起（「こんなお悩みありませんか？」）
- Before / After
- 機能紹介（テキスト + 画面モック、交互レイアウト）
- インタラクティブデモ（スクロールで自動再生）
- 数字で見る成果
- FAQ（アコーディオン）
- お問い合わせフォーム（Tally.so埋め込み）

## デザインは2パターン

サービスの種類に応じて、Claudeが自動で判断して提案してくれます。

**Tone A：ビジネスSaaS型**
BtoB・業務ツール向け。白背景にブルーのアクセントカラー、macOS風の画面モックが特徴。クリーンで堅実なデザイン。

**Tone B：ライフスタイル / D2C型**
BtoC・アプリ・ヘルスケア向け。背景にぼんやり浮かぶカラフルなオーブ、すりガラス風のカードが特徴。Apple的で洗練されたデザイン。

## 前提条件

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)がインストール済みであること
- Git / GitHub CLIが使えること（デプロイする場合）

## インストール方法

ターミナルで以下を実行するだけです:

```bash
git clone https://github.com/courtokuyama/create-lp-skill ~/.claude/skills/create-lp
```

これで `~/.claude/skills/create-lp/` にファイルが配置されます。Claude Codeが自動で認識するので、他に設定は不要です。

## 使い方

Claude Codeを開いて、スラッシュコマンドを打つだけ:

```
/create-lp my-service
```

`my-service` の部分にはプロジェクト名を入れてください（フォルダ名になります）。

あとはClaudeが順番に聞いてくるので、答えていけばLPが完成します。

### フロー

1. **ヒアリング** — サービスの内容、ターゲット、課題、機能を聞かれる
2. **構成設計** — セクション構成とデザイントーンの提案が来る。OKならそのまま、修正があれば伝える
3. **実装** — HTMLが自動生成される
4. **レビュー** — ブラウザで確認。「ここ直して」と言えば修正してくれる
5. **デプロイ** — GitHub Pagesに公開（任意）

## ファイル構成

```
create-lp/
├── SKILL.md          スキル本体。5フェーズのフロー定義
├── guidelines.md     デザインガイドライン。設計原則・2トーンの仕様・コンポーネント集
├── template.html     ベースHTML。CSS・JSが全部入り。これをコピーして使う
└── README.md         このファイル
```

## アップデート

```bash
cd ~/.claude/skills/create-lp && git pull
```
