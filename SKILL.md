---
name: create-lp
description: LP（ランディングページ）を新規作成するスキル。ヒアリングから構成設計、実装、デプロイまでの全フローを実行する。
argument-hint: [project-name]
disable-model-invocation: true
---

# LP作成スキル

デザインガイドラインに従ってLPを作成する。起動時に以下を読み込むこと:
- [guidelines.md](guidelines.md) — 設計原則・デザイントーン・コンポーネント仕様
- [template.html](template.html) — ベーステンプレート（Tone A: SaaS型）をコピーして使う

## Phase 1: ヒアリング（情報収集）

ユーザーに以下を聞く。すでに分かっている情報はスキップ。

### 必須情報
1. **サービス名** — LP上での表示名
2. **一言で何をするサービスか** — ヒーローのキャッチコピーの元になる
3. **ターゲットユーザー** — 誰の課題を解決するか
4. **主要な課題（3つ）** — 「お悩みセクション」に使う
5. **主要機能（3〜5つ）** — Pointセクションに使う。各機能のベネフィットも
6. **競合との違い** — 差別化ポイント
7. **ブランドカラー** — 指定がなければ提案する（1色のみ）
8. **CTA（何をさせたいか）** — ウェイトリスト登録、問い合わせ、無料トライアル等
9. **フォームURL** — Tally.so等の外部フォームURL。なければ作成を提案

### トーン選択（ヒアリング中に判断）
サービス内容から**Tone A（ビジネスSaaS型）**か**Tone B（ライフスタイル/D2C型）**を判断する。
- BtoB・業務ツール・SaaS → **Tone A**（CFManager系: ソリッド背景、macOSモック）
- BtoC・ヘルスケア・アプリ・プレミアム → **Tone B**（TUUN系: オーブ背景、グラスモーフィズム）
- ユーザーが迷っている場合はPhase 2で両方の方針を提示して選んでもらう

### あれば嬉しい情報
- 参考サイト or 好きなデザインテイスト
- 具体的な数字・KPI（デモで使う）
- ロゴ（SVGが理想）
- OGP画像
- 独自ドメインの有無
- 営業資料（service-doc）も必要か

## Phase 2: 構成設計

ヒアリング結果をもとに、以下を**テキストベースで**ユーザーに提示して合意を取る。

```
## LP構成案: [サービス名]

### セクション構成
1. ナビ: [ロゴ] + [メニュー項目] + [CTAボタン文言]
2. ヒーロー: [キャッチコピー案] / [サブコピー案]
3. 課題提起: [3つの課題]
4. 解決策: [Before/After or ソリューション概要]
5. 機能概要: [3〜5つのポイントカード]
6. 機能詳細: [各ポイントの説明 + モック概要]
7. デモ: [何をアニメーションさせるか]
8. 数字: [KPI 3-4個]
9. FAQ: [想定Q&A 4-6個]
10. CTA: [最終コール文言 + フォーム]

### デザイン方針
- ブランドカラー: [色コード]
- トーン: [例: プロフェッショナル / カジュアル / テック]
- 参考に近いスタイル: [TUUNクラブ型 / CFManager型 / カスタム]
```

ユーザーの合意を得てからPhase 3へ。修正があれば反映して再提示。

## Phase 3: 実装

### 3-1. プロジェクトセットアップ
```bash
mkdir -p ~/Desktop/$ARGUMENTS
cd ~/Desktop/$ARGUMENTS
git init
```

### 3-2. HTML作成
[template.html](template.html)をコピーして`index.html`を作成。`{{PLACEHOLDER}}`を実データに置換していく。

**Tone別の作業**:
- **Tone A（SaaS型）**: template.htmlをそのまま使う。`--accent`をブランドカラーに変更
- **Tone B（ライフスタイル型）**: template.htmlをベースに以下を差し替え:
  1. `:root`のCSS変数をTone B用に変更（guidelines.md §8参照）
  2. フォントをneue-haas-grotesk-display + Hiragino系に変更
  3. `<body>`直後に`.bg-orbs`オーブ要素を追加
  4. `.screen-mock`を`.glass-card`に置換（モック不要なケース）
  5. セクション区切りの`border-top`を削除（余白のみで区切る）
  6. `.animate`クラスとIntersectionObserver JSを追加

**共通で必ず守ること**:
- CSS変数でテーマ管理（色の直書き禁止）
- モバイル対応（`@media(max-width:768px)`）
- IntersectionObserverでデモ自動開始
- Tally.soフォーム埋め込み or mailto
- `<div>`の開きと閉じの数が一致していることを確認

### 3-3. 確認
作成後、ブラウザで開いて確認を促す:
```bash
open ~/Desktop/$ARGUMENTS/index.html
```

## Phase 4: レビュー＆修正

ユーザーにブラウザで確認してもらい、フィードバックを受けて修正する。

**よくある修正パターン**:
- テキストの改行位置調整（`<br>`追加）
- モバイルでの表示崩れ修正
- 色味の調整（`--accent`変更）
- セクションの順序変更
- モックの内容変更

修正のたびに`open`コマンドで再確認を促す。

## Phase 5: デプロイ

### GitHub Pages
```bash
cd ~/Desktop/$ARGUMENTS
git add index.html
git commit -m "initial LP"
gh repo create $ARGUMENTS --public --source=. --push
```
GitHub Settings → Pages → main branch → Save を案内。

### 営業資料が必要な場合
- `service-doc.html`を別途作成（1280x720スライド形式）
- Puppeteerで PDF生成

## 注意事項

- **ユーザーの判断を仰ぐ**: Phase 2の構成、Phase 4のレビューでは必ず確認を取る
- **過剰にしない**: ユーザーが求めていない機能やセクションを勝手に追加しない
- **正直さ**: フェイクデータ、嘘の実績、架空のレビューは絶対に使わない
- **速度重視**: 完璧を目指すより、まず動くものを出して修正サイクルを回す
- **日本語の自然さ**: 翻訳調の日本語にならないように注意。口語的でOK
