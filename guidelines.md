# LP Design Guidelines — Court's Style

## 1. 設計原則（Design Principles）

### 正直さ（Honesty First）
- フェイクの社会的証明を絶対に使わない（「1,000社導入」等の嘘数字、偽ロゴ、偽レビュー）
- 数字を出すなら裏付けのあるもの。なければ出さない
- ローンチ前なら「事前登録」「ウェイトリスト」で正直にポジショニング

### メリハリ（Contrast & Hierarchy）
- 大きい数字・見出しは極太（800）、説明は細め（300-400）で明確にコントラストを出す
- 重要なKPIや数字はclamp()で最低でも32px以上、できれば48px+
- セクション間は十分な余白（padding: 80px〜120px）
- 背景色の切り替え（白→薄灰→濃紺→白）でリズムを作る

### Anti-AI感（Don't Look AI-Generated）
- 一般的な「AIっぽい」デザイン（グラデーション多用、ジェネリックアイコン、テンプレ感）を避ける
- 要素は少なく、余白を多く。情報密度を下げる
- 日本語の自然な改行位置を意識（`<br>`で手動調整OK）
- デフォルトの絵文字やアイコンは使わず、SVGかテキストで代替

### モバイル対応（Mobile Simplification）
- PCの複雑なレイアウトをそのまま縮小しない
- モバイルでは列を減らす、非本質的な装飾を非表示にする
- `@media(max-width:768px)`で明示的にfont-size、padding、gridカラムを再定義
- ナビはハンバーガーメニューではなくCTAボタンのみに簡略化でもOK

### 速度（Performance）
- 外部リソースは最小限（Google Fontsのみ許容）
- 画像はインラインSVGかbase64 data URIで埋め込み
- JSライブラリ不使用。vanilla JSのみ
- ファイル数は原則1（index.html）。CSS・JSもすべてインライン

## 2. コードアーキテクチャ

### 単一HTMLファイル構成
```
index.html
├── <head>
│   ├── meta（SEO + OGP）
│   ├── favicon（inline SVG data URI）
│   ├── Google Fonts <link>
│   └── <style>（全CSSここに）
├── <body>
│   ├── ナビゲーション
│   ├── 各セクション（HTMLのみ）
│   └── <script>（全JSここに、</body>直前）
```

### CSS変数によるテーマ管理
```css
:root {
  --bg: #ffffff;
  --bg-alt: #f5f6f8;       /* セクション交互背景 */
  --bg-dark: #1a1e2e;      /* 濃い背景セクション */
  --text: #1e2330;
  --text-sub: #555e6e;     /* 補足テキスト */
  --text-light: #8891a0;   /* 最も薄いテキスト */
  --accent: #2563eb;       /* ブランドカラー */
  --accent-light: #eef2ff; /* アクセントの薄い版 */
  --accent-dark: #1a4fcc;  /* ホバー用 */
  --border: #e5e7eb;
  --green: #0d9f6e;
  --red: #dc3545;
  --radius: 8px;
  --radius-lg: 16px;
  --max-w: 1100px;
}
```
- プロジェクトごとに`--accent`を変えるだけでブランドカラーが変わる設計
- 色はCSS変数経由で参照。直書き禁止（`color: #2563eb` ではなく `color: var(--accent)`）

### フォント構成
- 日本語: `Noto Sans JP`（300, 400, 500, 600, 700, 800）
- 英数字/コード: `DM Mono`（400, 500）
- フォールバック: `-apple-system, BlinkMacSystemFont, sans-serif`

### リセット＆ベース
```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body { font-size: 15px; line-height: 1.8; -webkit-font-smoothing: antialiased; }
```

## 3. セクション構成テンプレート

### 必須セクション（この順番で）
1. **ナビ**（固定、backdrop-filter blur、CTAボタン付き）
2. **ヒーロー**（キャッチコピー + サブコピー + CTA + プロダクトモック）
3. **課題提起**（「こんなお悩みありませんか？」）
4. **解決策概要**（Before/After or ソリューション説明）
5. **機能一覧（概要）**（3〜5つのポイントカード、横並び）
6. **機能詳細（各ポイント）**（左テキスト・右モック、交互にreverse）
7. **デモ or 動くUI**（インタラクティブ要素、IntersectionObserverで自動開始）
8. **数字で見る成果**（KPI 3-4個、大きい数字）
9. **対象ユーザー or 比較表**
10. **FAQ**（アコーディオン）
11. **CTA（最終）**（フォーム埋め込み or リンク）
12. **フッター**（最小限の情報）

### 各セクションの背景パターン
```
ナビ    : 白（半透明 + blur）
ヒーロー : 白
課題提起 : 薄灰（--bg-alt）
解決策   : 白
機能概要 : 白
機能詳細 : 白（区切り線で分離）
デモ     : 濃紺（--bg-dark）→ テキストは白
数字     : 白 or 薄灰
FAQ      : 白
CTA      : 濃紺（--bg-dark）
フッター : 白 or 濃紺
```

## 4. UIコンポーネントパターン

### macOSスタイルのプロダクトモック
```html
<div class="screen-mock">
  <!-- ウィンドウバー -->
  <div class="screen-titlebar">
    <span class="dot red"></span>
    <span class="dot yellow"></span>
    <span class="dot green"></span>
    <span class="screen-url">app.example.com/dashboard</span>
  </div>
  <!-- コンテンツ -->
  <div class="screen-body">...</div>
</div>
```
- 3つのドット（赤・黄・緑）は必須
- URLバーでプロダクトの実在感を出す
- `border-radius: var(--radius-lg)`、`box-shadow`で浮遊感
- タブ切り替えUIも可（`.screen-tab-item`で切り替え）

### フローティングカード（ヒーローの装飾）
```html
<div class="float-card" style="top:XX%; left:XX%;">
  <span class="fc-val">¥12,340,000</span>
  <span class="fc-sub">今月の売上</span>
</div>
```
- ヒーローのモック周辺に配置して「動いてる感」を出す
- `animation: float 6s ease-in-out infinite`で微動

### ステータスバッジ
```css
.badge { font-size: 10px; padding: 2px 8px; border-radius: 99px; font-weight: 600; }
.badge-api { background: #dbeafe; color: #1e40af; }
.badge-csv { background: #f3e8ff; color: #7c3aed; }
.badge-done { background: #dcfce7; color: #15803d; }
```

### CTAボタン
```css
.btn-primary {
  background: var(--accent); color: #fff; border: none;
  padding: 16px 40px; border-radius: 99px;
  font-size: 16px; font-weight: 700;
  transition: background 0.2s, transform 0.2s;
}
.btn-primary:hover {
  background: var(--accent-dark); transform: translateY(-2px);
}
```
- 角丸は`99px`で完全にピル型
- ホバーで微かに浮く（`translateY(-2px)`）

### FAQアコーディオン
- CSSのみ（`<details>` + `<summary>`）、JSなしで実現可能
- または`max-height: 0` → `max-height: XXpx`のトランジションで

## 5. インタラクション

### IntersectionObserverによるデモ自動開始
```javascript
if('IntersectionObserver' in window){
  var obs = new IntersectionObserver(function(entries){
    entries.forEach(function(e){
      if(e.isIntersecting && !autoStarted){
        autoStarted = true;
        setTimeout(startAnimation, 600);
      }
    });
  }, {threshold: 0.4});
  obs.observe(document.getElementById('demo'));
}
```
- デモセクションがビューポートの40%以上見えたら自動開始
- 一度だけ実行（`autoStarted`フラグ）

### アニメーションの原則
- `setTimeout`チェーンでステップバイステップ
- 各ステップ300-500ms間隔（速すぎず遅すぎず）
- CSSトランジション: `opacity 0→1`, `transform translateY(8px→0)`
- ログ表示系は`scrollTop = scrollHeight`で自動スクロール
- ボタンはパルスアニメーション（`.demo-btn-pulse`）で「押して」感を演出

### フォーム埋め込み
- Tally.soのiframe埋め込みが標準（`data-tally-*`属性）
- フォームセクションは`min-height`を確保してレイアウトシフト防止

## 6. テキスト・コピー

### キャッチコピーのルール
- 1行は日本語で15〜20文字以内
- 体言止めまたは「〜する」で終わる
- 具体的な数字があれば入れる
- 技術用語よりベネフィット優先

### サブコピー
- ヒーロー直下: 1〜2行、400ウェイト、`--text-sub`色
- 各セクション冒頭: `letter-spacing: 0.15em; text-transform: uppercase`のラベル

### 数字の見せ方
```html
<span style="font-size:clamp(36px,5vw,56px); font-weight:800; color:var(--accent);">
  92<span style="font-size:0.5em;">%</span>
</span>
<div style="font-size:14px; color:var(--text-sub);">自動照合率</div>
```
- 数字は極大、単位は数字の半分サイズ
- ラベルは小さく、数字の下に配置

## 7. デプロイ

### GitHub Pages
```bash
git add index.html
git commit -m "update LP"
git push origin main
```
- リポジトリ名がそのままサブドメイン（`username.github.io/repo-name`）
- 独自ドメインはCNAMEファイルで設定

### PDF資料生成（Puppeteer）
```javascript
const puppeteer = require('puppeteer');
const browser = await puppeteer.launch({
  executablePath: '/Applications/Google Chrome.app/Contents/MacOS/Google Chrome',
  headless: 'new'
});
const page = await browser.newPage();
await page.goto('file:///path/to/service-doc.html', {waitUntil: 'networkidle0'});
await page.pdf({
  path: 'output.pdf',
  width: '1280px', height: '720px',
  printBackground: true, preferCSSPageSize: true
});
```
- service-doc.htmlは別ファイル（スライド形式、1280x720px固定）
- `page-break-after: always`で改ページ制御

## 8. デザイントーン（Tone Variants）

LPのジャンルに応じて2つのトーンを使い分ける。共通の設計原則（§1）とセクション構成（§3）は同じ。変わるのは**色・フォント・装飾・質感**。

### Tone A: ビジネスSaaS型（CFManager系）
- **用途**: BtoB、経理・業務系、SaaS、ツール紹介
- **キーワード**: 信頼、堅実、クリーン、読みやすさ
- **ベースカラー**: 白背景 + 濃紺セクション
- **アクセント**: ブルー系（`#2563eb`）
- **フォント**: `Noto Sans JP` + `DM Mono`（Google Fonts）
- **装飾**: 控えめ。macOSモック、フローティングカードが主役
- **背景**: ソリッドカラー（白 / 薄灰 / 濃紺）。グラデーションなし
- **カード**: `border: 1px solid var(--border)` + `box-shadow`。不透明
- **ボタン**: ピル型（`border-radius: 99px`）、ソリッドカラー
- **見出し**: weight 800、letter-spacing: -0.5px
- **セクション区切り**: `border-top` or 背景色切り替え

```css
/* Tone A: ビジネスSaaS */
:root {
  --bg: #ffffff; --bg-alt: #f5f6f8; --bg-dark: #1a1e2e;
  --text: #1e2330; --text-sub: #555e6e; --text-light: #8891a0;
  --accent: #2563eb; --accent-light: #eef2ff; --accent-dark: #1a4fcc;
  --border: #e5e7eb;
  --radius: 8px; --radius-lg: 16px;
}
```

### Tone B: ライフスタイル / D2C型（TUUN系）
- **用途**: ヘルスケア、D2C、アプリ紹介、プレミアム感のあるサービス
- **キーワード**: 洗練、未来的、ミニマル、Apple的
- **ベースカラー**: 白背景 + グラスモーフィズム
- **アクセント**: スカイブルー（`#5AC8FA`）
- **フォント**: `neue-haas-grotesk-display`（Adobe）+ `Hiragino Kaku Gothic ProN`（システム）
- **装飾**: 背景オーブ（ぼかし円、5色グラデーション、20-35秒で浮遊）
- **カード**: グラスモーフィズム（`backdrop-filter: blur(20px) saturate(180%)`、半透明背景、`inset box-shadow`）
- **ボタン**: 角丸（`12-24px`）、ソリッド or ガラス
- **見出し**: weight 700、letter-spacing: -0.02em、line-height: tight
- **セクション区切り**: 余白のみ（線なし）。オーブが通底して統一感

```css
/* Tone B: ライフスタイル / D2C */
:root {
  --bg: #ffffff; --bg-alt: #F2F5F8;
  --text: #1D1D1F; --text-sub: #666666; --text-light: #999999;
  --accent: #5AC8FA; --accent-hover: #48B0E0;
  --glass-bg: rgba(242,245,248,0.7); --glass-border: rgba(255,255,255,0.5);
  --shadow: rgba(0,0,0,0.08); --shadow-strong: rgba(0,0,0,0.15);
  --radius: 12px; --radius-lg: 24px;
}
```

#### Tone B 固有コンポーネント: 背景オーブ
```css
.bg-orbs { position: fixed; inset: 0; z-index: 0; pointer-events: none; overflow: hidden; }
.orb {
  position: absolute; border-radius: 50%;
  filter: blur(60px); opacity: 0.5;
  animation: orbFloat 25s ease-in-out infinite;
}
.orb-1 { width: 400px; height: 400px; top: -10%; left: -5%;
  background: linear-gradient(135deg, #5AC8FA, #007AFF); animation-duration: 20s; }
.orb-2 { width: 350px; height: 350px; top: 30%; right: -8%;
  background: linear-gradient(135deg, #AF52DE, #5856D6); animation-duration: 28s; animation-delay: -5s; }
.orb-3 { width: 300px; height: 300px; bottom: 10%; left: 20%;
  background: linear-gradient(135deg, #FF2D55, #FF3B30); animation-duration: 32s; animation-delay: -10s; }

@keyframes orbFloat {
  0%, 100% { transform: translate(0, 0) scale(1); }
  25% { transform: translate(50px, -40px) scale(1.08); }
  50% { transform: translate(-30px, 30px) scale(0.92); }
  75% { transform: translate(60px, 15px) scale(1.05); }
}
```

#### Tone B 固有コンポーネント: グラスカード
```css
.glass-card {
  background: var(--glass-bg);
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-lg);
  box-shadow: 0 8px 32px var(--shadow), 0 1px 0 0 rgba(255,255,255,0.5) inset;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.glass-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 16px 48px var(--shadow-strong), 0 1px 0 0 rgba(255,255,255,0.5) inset;
}
```

#### Tone B 固有: スクロールアニメーション
```css
.animate { opacity: 0; transform: translateY(20px); transition: opacity 0.8s ease, transform 0.8s ease; }
.animate.visible { opacity: 1; transform: translateY(0); }
```
```javascript
// IntersectionObserverで.animateクラスに.visibleを付与
var observer = new IntersectionObserver(function(entries){
  entries.forEach(function(e){
    if(e.isIntersecting) e.target.classList.add('visible');
  });
}, {threshold: 0.1});
document.querySelectorAll('.animate').forEach(function(el){ observer.observe(el); });
```

### トーン選択の判断基準
| 判断軸 | Tone A（SaaS） | Tone B（ライフスタイル） |
|---|---|---|
| ターゲット | 経理・管理部門・BtoB | 個人・健康・美容・BtoC |
| プロダクト | 業務ツール・ダッシュボード | アプリ・デバイス・サービス |
| 信頼の表現 | データ・数字・Before/After | 世界観・質感・ブランド体験 |
| モック | macOSウィンドウ型 | アプリスクリーン or グラスカード |
| 背景 | ソリッド（白/灰/紺） | オーブ + グラスモーフィズム |
| 参考サイト | Sairu, Initial | Sapphire, Neko Health, Apple |

ユーザーが明示しない場合はサービス内容から判断し、Phase 2で提案する。

## 9. アンチパターン（やってはいけないこと）

- テンプレサイト感のあるアイコンパック使用
- ジェネリックなストックフォト
- 「お客様の声」セクション（実データがない段階で）
- CSS overflow: hiddenの雑な使用（モバイルでコンテンツが切れる）
- フォントサイズの直書き（clamp()を使う）
- 複数ファイルに分割（原則1ファイル）
- JSフレームワーク（React, Vue等）の使用
- 色の直書き（CSS変数を使う）
- 装飾過多（グラデーション背景、パーティクル、3Dエフェクト等）
- 画像ファイルの外部参照（読み込み失敗リスク）
