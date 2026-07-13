# Toshi Design System

Apple Developer Documentation のデザイン言語をベースにした、調査レポート・技術ドキュメント・HTML 成果物のための自分専用デザインシステム。

**公開 URL**: https://branch10480.github.io/design-system/ （GitHub Pages。main へ push すると自動デプロイ）

## ファイル

- [`design-system.html`](./design-system.html) — デザインシステム本体。リファレンスであると同時に「コピペ元」として機能する単一 HTML
- [`single-page.html`](./single-page.html) — シングルページパターンのテンプレート。ナビ・サイドバーなしの 1 カラム + フローティングアンカーメニュー構成。レポートを 1 枚で公開するときはこれをコピーして使う
- [`index.html`](./index.html) — GitHub Pages のルート URL から `design-system.html` へリダイレクトするだけのランディング
- [`fonts/`](./fonts/) — リポジトリ同梱の UDEV Gothic 35LG（woff2 3 ウェイト + OFL ライセンス `LICENSE-UDEVGothic.txt`）。コード・mono 表示用に `@font-face` で参照する。旧 JetBrains Mono の woff2 + `OFL.txt` は過去成果物が絶対 URL で参照し続けているため残置

## 特徴

- **単一ファイル + 同梱フォントで完結** — 外部 CDN への依存なし。フォントはシステムフォントとリポジトリ同梱の UDEV Gothic 35LG（英数字 = JetBrains Mono 由来 / 日本語 = BIZ UD ゴシック由来）のみ。`:root` のトークンブロックをコピーすれば新しい成果物が同じ見た目になる（他リポジトリの成果物からは GitHub Pages の絶対 URL `https://branch10480.github.io/design-system/fonts/…` でフォント参照可）
- **ライト / ダーク自動対応** — 色はすべて CSS 変数（トークン）経由。OS の外観設定に追従し、ナビ右上のトグルで手動切り替え（自動 / ライト / ダーク）も可能。ダークは `#111111` 基調、ライトは文字色 `#0D0D0D` を採用し、本文コントラストを light/dark 双方で約 18〜19:1 に統一する（v0.7.0）
- **Apple Docs の語彙** — [swift-docc-render](https://github.com/swiftlang/swift-docc-render)（developer.apple.com ドキュメントの実レンダラー）のカラーパレットと、eyebrow / abstract / aside / availability などのレイアウト語彙を踏襲
- **SPA 構成** — ハッシュルーティング（`#/カテゴリ/セクション`）でトップのカテゴリカード → 詳細ページ（左サイドバー + コンテンツの 2 カラム）へ遷移。ページ内はフローティングアンカーメニュー（スクロール連動）で移動できる

## 構成

| ルート | 内容 |
|---|---|
| `#/` | トップ。カテゴリカード一覧 |
| `#/foundations` | 概要 / カラー / タイポグラフィ / スペーシング / 角丸 / エレベーション |
| `#/components` | ボタン / バッジ / カード / Aside / 要点ボックス（Key Takeaways）/ コード（コピーボタン付き）/ テーブル / タブ / リスト |
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

現在の版は `v0.7.0`。dark の `--bg` を `#181818` → `#111111`、dark の `--text` を `#D0D3D8` → `#FCFCFC`、light の `--text` を `#1D1D1F` → `#0D0D0D` に変更し、light/dark 双方の本文コントラストを約 18〜19:1 に統一した版。連動して dark の surface（`--bg-alt` / `--bg-elev` / `--bg-code` / `--separator`）と secondary/tertiary text・tint・status colors・syntax colors は、旧 `--bg` に対するコントラスト比を保ったまま新 `--bg` 基準へ再計算した。v0.6.0 は mono フォントを JetBrains Mono から UDEV Gothic 35LG（英数字 = JetBrains Mono 由来 / 日本語 = BIZ UD ゴシック由来、OFL）へ移行した版。日本語もコード・mono 表示で同一フォントになり、woff2 は 3 ウェイト同梱（約 1.8MB/個 — 日本語グリフ内蔵のため旧 JBM より大きいが `font-display: swap` で描画は妨げない）。v0.5.0 は読みやすさ（認知負荷の低減）を底上げした版。テキストトークンを WCAG AA（本文 ≥4.5:1）へ微調整し、本文の 1 行あたり文字数（measure）に上限を設け、レポート冒頭に置く要点ボックス（`.summary` / Key Takeaways）を追加した。過去の版ではダークトークン、リンク色、シンタックスカラー、mono 表示サイズを読みやすさ優先で調整している。
