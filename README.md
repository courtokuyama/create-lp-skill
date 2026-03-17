# /create-lp — Claude Code LP作成スキル

Claude Codeのカスタムスキル。`/create-lp project-name` でLP（ランディングページ）をゼロから作成する。

## 何ができるか

5フェーズで完結するLP作成フロー:

1. **ヒアリング** — サービス名、ターゲット、課題、機能、CTAを聞く
2. **構成設計** — セクション構成 + デザイントーンを提案、合意を取る
3. **実装** — 単一HTML（CSS/JS埋め込み）を自動生成
4. **レビュー** — ブラウザで確認、フィードバックを受けて修正
5. **デプロイ** — GitHub Pagesに公開

## デザイントーン

サービスの性質に応じて2つのトーンを使い分ける:

| | Tone A: ビジネスSaaS型 | Tone B: ライフスタイル/D2C型 |
|---|---|---|
| 向き | BtoB・業務ツール | BtoC・アプリ・プレミアム |
| 背景 | ソリッド（白/灰/濃紺） | オーブ + グラスモーフィズム |
| モック | macOSウィンドウ型 | グラスカード |
| 参考 | CFManager | TUUN Club |

## インストール

```bash
git clone https://github.com/courtokuyama/create-lp-skill ~/.claude/skills/create-lp
```

## 使い方

Claude Codeで:

```
/create-lp my-service-name
```

## ファイル構成

```
├── SKILL.md          — スキル本体（5フェーズフロー定義）
├── guidelines.md     — デザインガイドライン（原則・トーン・コンポーネント仕様）
└── template.html     — ベーステンプレートHTML（Tone A、コピーして使う）
```

## 設計原則

- **正直さ** — フェイクの社会的証明を使わない
- **メリハリ** — 大きい数字は極太、説明は細め
- **Anti-AI感** — テンプレ感のないデザイン
- **単一ファイル** — HTML/CSS/JS全部1ファイル
- **速度** — 外部リソース最小限、JSライブラリなし
