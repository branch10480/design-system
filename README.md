# Toshi Design System

Apple Developer Documentation のデザイン言語をベースにした、調査レポート・技術ドキュメント・HTML 成果物のための自分専用デザインシステム。

**公開 URL**: https://branch10480.github.io/design-system/ （GitHub Pages。main へ push すると自動デプロイ）

## ファイル

- [`design-system.html`](./design-system.html) — デザインシステム本体。リファレンスであると同時に「コピペ元」として機能する単一 HTML
- [`single-page.html`](./single-page.html) — シングルページパターンのテンプレート。ナビ・サイドバーなしの 1 カラム + フローティングアンカーメニュー構成。レポートを 1 枚で公開するときはこれをコピーして使う
- [`index.html`](./index.html) — GitHub Pages のルート URL から `design-system.html` へリダイレクトするだけのランディング
- [`fonts/`](./fonts/) — リポジトリ同梱の UDEV Gothic 35LG（woff2 3 ウェイト + OFL ライセンス `LICENSE-UDEVGothic.txt`）。コード・mono 表示用に `@font-face` で参照する。旧 JetBrains Mono の woff2 + `OFL.txt` は過去成果物が絶対 URL で参照し続けているため残置。**mono の第一候補は Apple SF Mono だが、Apple のフォントライセンス上 woff2 として同梱できないため system stack として参照するのみで、ここには含まれない**（同梱するのはあくまで UDEV Gothic だけ）

## 特徴

- **単一ファイル + 同梱フォントで完結** — 外部 CDN への依存なし。フォントはシステムフォントとリポジトリ同梱の UDEV Gothic 35LG（英数字 = JetBrains Mono 由来 / 日本語 = BIZ UD ゴシック由来）のみ。**ただし mono の第一候補である Apple SF Mono だけは例外で、ライセンス上同梱できないため system stack として参照する**（SF Mono 未インストールの環境では UDEV Gothic にフォールバックする）。`:root` のトークンブロックをコピーすれば新しい成果物が同じ見た目になる（他リポジトリの成果物からは GitHub Pages の絶対 URL `https://branch10480.github.io/design-system/fonts/…` でフォント参照可）
- **ライト / ダーク自動対応** — 色はすべて CSS 変数（トークン）経由。OS の外観設定に追従し、ナビ右上のトグルで手動切り替え（自動 / ライト / ダーク）も可能。ダークは純黒ではなく `#181818` 基調の raised black を採用し、長文・コード・表を読み続けても疲れにくい階調にする
- **Apple Docs の語彙** — [swift-docc-render](https://github.com/swiftlang/swift-docc-render)（developer.apple.com ドキュメントの実レンダラー）のカラーパレットと、eyebrow / abstract / aside / availability などのレイアウト語彙を踏襲
- **SPA 構成** — ハッシュルーティング（`#/カテゴリ/セクション`）でトップのカテゴリカード → 詳細ページ（左サイドバー + コンテンツの 2 カラム）へ遷移。ページ内はフローティングアンカーメニュー（スクロール連動）で移動できる

## 構成

| ルート | 内容 |
|---|---|
| `#/` | トップ。カテゴリカード一覧 |
| `#/foundations` | 概要 / カラー / タイポグラフィ / スペーシング / 角丸 / エレベーション |
| `#/components` | ボタン / バッジ / カード / Aside / 要点ボックス（Key Takeaways）/ コード（コピーボタン付き）/ Before After 比較 / テーブル / タブ / リスト |
| `#/patterns` | ページヘッダ / ナビゲーション / 2 カラムレイアウト / フローティングメニュー / シングルページ / フッター |
| `#/guidelines` | 原則 / 使い方 / Do・Don't / バージョニング（changelog） |

## 使い方

1. `design-system.html` をブラウザで開く（`open design-system.html`）
2. 新しい成果物を作るときは `:root` のトークン一式、dark の 2 ブロック（`@media (prefers-color-scheme: dark)` / `[data-theme="dark"]`）、テーマ初期化スクリプトを**セットで**コピーする
3. 必要なコンポーネントの CSS + HTML を持っていく

Auto テーマはブラウザの `prefers-color-scheme` を初期値にしつつ、ローカルアプリ側に `/api/appearance` がある場合は実際の macOS `AppleInterfaceStyle` などを優先する。ブラウザの判定だけでは、Mac がダークでも localhost 側でライトになることがある。

詳細は `#/guidelines` を参照。

## バージョニング

semver で管理する。トークンの値変更は minor、トークン名の変更・削除は major（成果物側が名前で参照しているため breaking）。

現在の版は `v0.10.0`。**見出し以外のフォントサイズを 1pt 上げた**版。v0.8.0 の一律 90% 縮小で本文系が小さすぎたため、Headline 以下のテキストトークンと UI ラベル・コンポーネント内テキストだけを底上げした。換算ルールは「**.5 は整数へ切り上げてから +1px**」で、結果として `.5` 刻みは全廃され整数だけの尺度になる。代表値は 15.5→17 / 14.5→16 / 13.5→15 / 12.5→14 / 12→13 / 11.5→13 / 11→12 / 10→11px。本文（`body` / `.doc-section p`）は 13.5→**15px**、リード文（`.doc-header .abstract`）は 15.5→**17px**、コードブロック（`pre.listing`）は 12.5→**14px**（`.ba-grid` 変種は 11.5→13px）、テーブルと mono も 12.5→14px。**据え置きは型スケールの Title 以上だけ** — hero `clamp(31px, 4.7vw, 47px)` / `.doc-header h1` `clamp(29px, 4.1vw, 40px)` / `h2` 25px / Title 3 19px / `.doc-section h3` 17px。逆に **Headline(15.5px) と本文スケールに置かれた小ラベル h4 は見出し扱いしない**（`.feature h3` 15.5→17 / `.topic-card h4` 14.5→16 / `.tone-sample h4` 13.5→15 / `.dodont h4` 12.5→14）。据え置くと本文 15px と同サイズか下回り、`.dodont h4` は本文 li より小さくなって階層が逆転するため。`em` 相対指定（`code.inline` の `0.92em`）と SVG の `font-size` 属性（viewBox スケール）は v0.8.0 と同じく据え置きで、色・余白・字間・行間は変更していない。v0.9.0 は mono フォントの第一候補を **Apple SF Mono** へ移行した版（`--font-mono` の先頭）。SF Mono は Apple のフォントライセンス上 woff2 として再配布・改変ができないため woff2 は同梱せず system stack として参照するのみで、同梱フォントは従来どおり UDEV Gothic 35LG のまま変更していない。並び順は `"SF Mono"` →（Safari / WebKit では `ui-monospace` が SF Mono に解決されるため未インストール Mac でも SF Mono で描ける。ただし Chrome on macOS の `ui-monospace` は Menlo に解決されるため `"SF Mono"` を 1 番手に置く意味がある）`ui-monospace` → `"SFMono-Regular"` → **UDEV Gothic 35LG**（SF Mono は日本語グリフを持たないため和文は per-glyph fallback でここが拾い、SF Mono 未インストール環境では欧文もここが担う）→ `Menlo` 以降（macOS 標準の最終保険）の順。`--code-weight` は 400 のまま変更していない（SF Mono には実体の Medium(500) があるが、UDEV Gothic に落ちる環境との重み感を揃えるため）。背景として、castle（個人 dotfiles）側で Ghostty / cmux / Orca / VS Code / Xcode / Codex.app のコードフォントを SF Mono へ統一したのに合わせた変更。v0.8.0 はサイト全体のフォントサイズを**一律 90%** に縮小した版。換算ルールは「旧値 × 0.9 → 0.5px 刻みに丸め（下限 10px）」で、見出しなど大きい値は整数丸め、`clamp()` は min / vw 係数 / max をすべて 0.9 倍する。代表値は 17→15.5 / 16→14.5 / 15→13.5 / 14→12.5 / 13→12 / 12→11 / 11→10px。本文は 17px → **15.5px**、コードブロック（`pre.listing`）は 15px → **13.5px**、テーブル内 mono は 14px → **12.5px**、hero は `clamp(34px, 5.2vw, 52px)` → `clamp(31px, 4.7vw, 47px)`。`em` / `%` / `rem` の相対指定と SVG の `font-size` 属性（viewBox スケール）は据え置きで、色・余白・字間・行間は変更していない。その後の調整で、リード文（`.doc-header .abstract`）は 17→15.5px、本文（`body`）は 15.5→**13.5px**、コードブロック（`pre.listing`）は 13.5→**12.5px**（`.ba-grid` 変種は 12.5→11.5px）へさらに一段下げた。v0.7.0 はコンテンツ幅を広げ、右端を 1 本に揃えた版。詳細ページの外枠を 1400px、コンテンツカラムを最大 1040px（右端のフローティングメニューに被らないところまで自動で伸びる可変幅）にし、single-page の `.article` とナビ・フッター・ホームも合わせて揃えた。あわせて v0.5.0 で入れた個別の measure（`.doc-section > p` の 70ch）を撤去し、**段落・リスト・aside・テーブル・コード・図をすべて同じ幅に置いて右端を揃える**方式へ変更した（行長は `--doc-max` 側で決める）。読み物ブロックだけ狭いと要素ごとに右端がズレてギザギザに見えるため。左右にコードを並べる比較や列の多い表が窮屈だった問題への対応でもある。コードブロック（`pre.listing`）は 14px → 15px に上げ、本文と同サイズにした。新コンポーネントとして `.ba-grid`（Before / After のコードを左右に並べる 2 カラム比較。900px 以下で縦積み）を追加した。v0.6.0 は mono フォントを JetBrains Mono から UDEV Gothic 35LG（英数字 = JetBrains Mono 由来 / 日本語 = BIZ UD ゴシック由来、OFL）へ移行した版。日本語もコード・mono 表示で同一フォントになり、woff2 は 3 ウェイト同梱（約 1.8MB/個 — 日本語グリフ内蔵のため旧 JBM より大きいが `font-display: swap` で描画は妨げない）。v0.5.0 は読みやすさ（認知負荷の低減）を底上げした版。テキストトークンを WCAG AA（本文 ≥4.5:1）へ微調整し、本文の 1 行あたり文字数（measure）に上限を設け、レポート冒頭に置く要点ボックス（`.summary` / Key Takeaways）を追加した。過去の版ではダークトークン、リンク色、シンタックスカラー、mono 表示サイズを読みやすさ優先で調整している。
