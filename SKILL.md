---
name: create-lp
description: LP（ランディングページ）＆ 営業資料（サービス詳細PDF）を作成するスキル。ヒアリングから構成設計、実装、デプロイまでの全フローを実行する。
argument-hint: [project-name]
disable-model-invocation: true
---

# LP & 営業資料 作成スキル

デザインガイドラインに従ってLP・営業資料を作成する。起動時に以下を読み込むこと:
- [guidelines.md](guidelines.md) — 設計原則・デザイントーン・コンポーネント仕様
- [template.html](template.html) — ベーステンプレート（Tone A: SaaS型）をコピーして使う

---

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
サービス内容から**Tone A（ビジネスSaaS型）**か**Tone B（ライフスタイル/D2C型）**か**Tone C（BPO×AI型）**を判断する。
- BtoB・業務ツール・SaaS → **Tone A**（CFManager系: ソリッド背景、macOSモック）
- BtoC・ヘルスケア・アプリ・プレミアム → **Tone B**（TUUN系: オーブ背景、グラスモーフィズム）
- BPO・コンサルティング・AI導入支援 → **Tone C**（気づけばAI系: ダーク背景、写真ヒーロー、比較表、フェーズ型サービス説明）
- ユーザーが迷っている場合はPhase 2で両方の方針を提示して選んでもらう

### あれば嬉しい情報
- 参考サイト or 好きなデザインテイスト
- 具体的な数字・KPI（デモで使う）
- ロゴ（SVGが理想）
- OGP画像
- 独自ドメインの有無
- **営業資料（service-doc）も必要か** ← 必ず聞く

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
10. CTA: [最終コール文言 + フォーム + 資料内容リスト]
```

ユーザーの合意を得てからPhase 3へ。修正があれば反映して再提示。

## Phase 3: LP実装

### 3-1. プロジェクトセットアップ
```bash
mkdir -p ~/Documents/$ARGUMENTS
cd ~/Documents/$ARGUMENTS
git init
```

### 3-2. HTML作成
[template.html](template.html)をコピーして`index.html`を作成。`{{PLACEHOLDER}}`を実データに置換していく。

**Tone別の作業**:
- **Tone A（SaaS型）**: template.htmlをそのまま使う。`--accent`をブランドカラーに変更
- **Tone B（ライフスタイル型）**: template.htmlをベースに差し替え（guidelines.md §8参照）
- **Tone C（BPO×AI型）**: guidelines.md §8-C参照。ダークヒーロー、比較表、フェーズ型カード

**共通で必ず守ること**:
- CSS変数でテーマ管理（色の直書き禁止）
- モバイル対応（`@media(max-width:768px)`）
- Tally.soフォーム埋め込み or mailto
- `<div>`の開きと閉じの数が一致していることを確認

### 3-3. CTAセクション設計（重要）
最終CTAセクションには必ず「資料でわかること」リストを入れる:
```html
<div style="background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.1);border-radius:12px;padding:24px 28px;margin-bottom:24px">
  <div style="font-size:14px;font-weight:700;color:rgba(255,255,255,.85);margin-bottom:14px">
    📄 資料でわかること
  </div>
  <ul>
    <li>● [資料に含まれる具体的な内容1]</li>
    <li>● [資料に含まれる具体的な内容2]</li>
    ...
  </ul>
</div>
```
- フォームの高さは十分に確保し、スクロールなしで全体が表示されるようにする
- Tally iframeは `height:700px` 以上、コンテナは `height:620px` 以上

### 3-4. 確認
```bash
open ~/Documents/$ARGUMENTS/index.html
```

## Phase 4: 営業資料（service-doc）作成

LPとセットで営業資料を作る。LPには載せきれない詳細情報を盛り込む。

### 4-1. フォーマット仕様
- **サイズ**: 1920x1080px（16:9横長スライド形式）
- **ファイル名**: `service-doc.html`（LP と同じディレクトリに配置）
- **総スライド数**: 17〜20枚

### 4-2. スライド構成（20枚、明朗会計AI資料準拠）

実績あるテンプレート。事例の数や料金プラン数はサービスに応じて増減OK。

```
p.1   カバー
      - サービス名（80px、左寄せ）
      - サブコピー（26px）
      - 会社名、メールアドレス、版数
      - 装飾: 右側にアクセントカラーの円グラデーション

p.2   目次（CONTENTS）
      - 6セクションを2×3グリッドで配置
      - 各項目に連番バッジ（アクセントカラー角丸正方形）+ セクション名 + 1行説明 + ページ番号（右端にp.3等）
      - 項目ごとにカード化（背景色 + ボーダー）

p.3   サービス概要（SERVICE OVERVIEW）
      - セクションラベル + タイトル（52px）+ リード文（22px）
      - 提供内容を4カードで横並び（番号バッジ + タイトル + 説明文）
      - 下段に対応領域を2×2のコンパクトカード（ドット + タイトル + 説明）

p.4   実績サマリー（RESULTS）
      - 背景色を変えて視覚的に区別（bg2）
      - 大きい数字4つを横並び（数字64px + 単位22px + ラベル + 説明文）
      - 下段にサブ指標4つ（28px数字 + ラベル、背景カード）

p.5   事例セクションタイトル
      - アクセントカラー全面背景（白文字）
      - セクションラベル + タイトル（72px）+ サブコピー
      - 下部に3つの数値サマリー（事例数 / 業種数 / 平均削減率）を横並び

p.6-15  導入事例 × 10社（左:背景画像38% / 右:詳細テキスト62%）
      → 詳細は「4-3. 事例スライドの設計ルール」参照

p.16  導入プロセス（PROCESS）
      - タイトル + リード文
      - 6ステップを3×2グリッドで配置
      - 各カード: ステップ番号（丸バッジ）+ 期間バッジ + タイトル + 説明文2-3行

p.17  技術基盤 & セキュリティ（TECHNOLOGY & SECURITY）
      - 背景色変更（bg2）
      - 6カードを3×2グリッド
      - 各カード: アイコン（テキスト型、48px角丸背景）+ タイトル横並び + 説明文2行

p.18  料金体系（PRICING）
      - タイトル + リード文
      - 3-4プランを横並び（price-card）
      - 人気プランに「人気」バッジ + 太枠（アクセントカラー）
      - 各カード: プラン名 + 価格（38px）+ 説明 + チェックリスト5項目
      - 下部に注意書き（税別、別途費用の説明）

p.19  品質保証（GUARANTEE & SUPPORT）
      - 背景色変更（bg2）
      - 4カードを2×2グリッド
      - 各カード: 番号バッジ + タイトル（20px）+ 説明文3行

p.20  エンドスライド
      - アクセントカラー全面背景（白文字）
      - タイトル（56px）+ サブコピー
      - CTAボタン（mailto:リンク、白背景 + アクセントカラー文字）
      - 会社名 + メールアドレス
```

**スライド間の背景パターン:**
```
p.1  グラデーション  p.6-15  白 (事例)
p.2  白              p.16    白
p.3  白              p.17    bg2
p.4  bg2             p.18    白
p.5  アクセント全面   p.19    bg2
                     p.20    アクセント全面
```
白とbg2を交互に使い、セクションの切れ目でアクセントカラー全面を挟む。

### 4-3. 事例スライドの設計ルール
```
┌────────────────────────────────────────────────┐
│  ┌──────────┐  ┌──────────────────────────────┐ │
│  │          │  │ [番号] [業種タグ] [企業規模]  │ │
│  │  背景    │  │                              │ │
│  │  画像    │  │ ★ 見出し（48px、。で改行）   │ │
│  │  38%幅   │  │                              │ │
│  │          │  │ [課題] [導入期間] [使用技術]  │ │
│  │ gradient │  │                              │ │
│  │ overlay  │  │ ▎導入前の課題                │ │
│  │          │  │  本文（18px、詳細に記述）     │ │
│  │          │  │                              │ │
│  │          │  │ ▎構築したソリューション       │ │
│  │          │  │  • 箇条書き3点               │ │
│  │          │  │                              │ │
│  │          │  │ ┃ 成果ヘッドライン（24px）   │ │
│  │          │  │ ┃ 成果の詳細説明             │ │
│  └──────────┘  └──────────────────────────────┘ │
│  [ロゴ]                              [ページ番号]│
└────────────────────────────────────────────────┘
```

**画像**: `<img>`タグではなく `background-image` + `background-size:cover` を使う（html2canvasの互換性のため）
**グラデーション**: 画像の右端にCSS疑似要素でフェードオーバーレイ
**テキスト**: 右側を80%以上テキストで埋める。スカスカにしない
**見出し**: `。`で改行（`<br>`挿入）。フォントサイズは他ページのセクションタイトルと同等（48px）

### 4-4. PDFダウンロード機能
**印刷ダイアログ（window.print）は使わない。** ユーザーがA4に変換されて困る。

```html
<!-- CDN読み込み -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
```

```javascript
async function downloadPDF(){
  var btn = document.getElementById('dlBtn');
  btn.disabled = true;
  var jsPDF = window.jspdf.jsPDF;
  var pdf = new jsPDF({
    orientation:'landscape', unit:'px',
    format:[1920,1080], hotfixes:['px_scaling'], compress:false
  });
  var slides = document.querySelectorAll('.slide');
  for(var i=0; i<slides.length; i++){
    var s = slides[i];
    var origT = s.style.transform;
    var origM = s.style.marginBottom;
    s.style.transform = 'none';
    s.style.marginBottom = '0';
    var canvas = await html2canvas(s, {
      width:1920, height:1080, scale:2,
      useCORS:true, allowTaint:true, logging:false,
      backgroundColor:'#ffffff', imageTimeout:15000
    });
    var img = canvas.toDataURL('image/png');
    if(i > 0) pdf.addPage();
    pdf.addImage(img, 'PNG', 0, 0, 1920, 1080, undefined, 'FAST');
    s.style.transform = origT;
    s.style.marginBottom = origM;
  }
  pdf.save('ファイル名.pdf');
  btn.disabled = false;
}
```

- `scale:2` でレティナ品質
- `image/png` で色味を正確に保持（JPEGは色が変わる）
- `compress:false` で圧縮による劣化を防止

### 4-5. PDFビューアーバー
ページ上部に固定のダークバーを配置:
```html
<div class="viewer-bar">
  <div class="viewer-bar-left">
    <div class="viewer-bar-icon">📄</div>
    <div class="viewer-bar-title">ファイル名.pdf</div>
    <div class="viewer-bar-sub">XX ページ / 会社名</div>
  </div>
  <div class="viewer-bar-right">
    <button class="dl-btn" onclick="downloadPDF()">
      ⬇ PDFをダウンロード
    </button>
  </div>
</div>
```
- 印刷時は `display:none`
- ボタンは生成中に `disabled` にして「生成中... (3/20)」と進捗表示

### 4-6. スライドのスケーリング（レスポンシブ表示）
```javascript
function scaleSlides(){
  var vw = window.innerWidth;
  var scale = Math.min(vw/1920, 1);
  var offsetX = (vw - 1920*scale) / 2;
  document.querySelectorAll('.slide').forEach(function(s){
    s.style.transform = 'translateX('+offsetX+'px) scale('+scale+')';
    s.style.marginBottom = (-(1080*(1-scale))+40)+'px';
  });
}
```
- `transform-origin: top left` が必須（top centerだと右が切れる）
- `html, body` に `overflow-x:hidden`

## Phase 5: レビュー＆修正

ユーザーにブラウザで確認してもらい、フィードバックを受けて修正する。

**よくある修正パターン**:
- テキストの改行位置調整（`<br>`追加）
- モバイルでの表示崩れ修正
- 色味の調整（`--accent`変更）
- セクションの順序変更
- 事例スライドの文字量調整（80%以上埋める）
- PDFの画質・色味確認

修正のたびに`open`コマンドで再確認を促す。

## Phase 6: デプロイ

### GitHub Pages
```bash
cd ~/Documents/$ARGUMENTS
git add index.html service-doc.html
git commit -m "LP + 営業資料"
gh repo create $ARGUMENTS --public --source=. --push
```
GitHub Pages を有効にする:
```bash
gh api repos/USERNAME/$ARGUMENTS/pages --method POST \
  -f build_type=legacy -f source='{"branch":"main","path":"/"}'
```

### LP から資料へのリンク
LPの最終CTAセクションのフォーム説明文に「資料でわかること」リストを入れ、ユーザーがフォーム送信後に資料ページURLを案内する運用にする。

## 注意事項

- **ユーザーの判断を仰ぐ**: Phase 2の構成、Phase 5のレビューでは必ず確認を取る
- **過剰にしない**: ユーザーが求めていない機能やセクションを勝手に追加しない
- **正直さ**: フェイクデータ、嘘の実績、架空のレビューは絶対に使わない
- **速度重視**: 完璧を目指すより、まず動くものを出して修正サイクルを回す
- **日本語の自然さ**: 翻訳調の日本語にならないように注意。口語的でOK
- **事例のリアリティ**: 導入事例は具体的な数字・業種・課題を入れて信憑性を出す。ただし担当者名は絶対に入れない
- **画像はbackground-image**: `<img>`+`object-fit:cover`はhtml2canvasで歪む。必ず`background-image`+`background-size:cover`を使う
- **PDFはPNG出力**: JPEGだと色味が変わる。`toDataURL('image/png')` + `scale:2`
