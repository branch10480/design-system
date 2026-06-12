# Toshi Design System

Apple Developer Documentation のデザイン言語をベースにした、調査レポート・技術ドキュメント・HTML 成果物のための自分専用デザインシステム。

## ファイル

- [`designsystem.html`](./designsystem.html) — デザインシステム本体。リファレンスであると同時に「コピペ元」として機能する単一 HTML

## 特徴

- **単一ファイル完結** — 外部 CSS / JS / Web フォントへの依存なし。`:root` のトークンブロックをコピーすれば新しい成果物が同じ見た目になる
- **ライト / ダーク自動対応** — 色はすべて CSS 変数（トークン）経由。OS の外観設定に追従し、ナビ右上のトグルで手動切り替え（自動 / ライト / ダーク）も可能
- **Apple Docs の語彙** — [swift-docc-render](https://github.com/swiftlang/swift-docc-render)（developer.apple.com ドキュメントの実レンダラー）のカラーパレットと、eyebrow / abstract / aside / availability などのレイアウト語彙を踏襲
- **SPA 構成** — ハッシュルーティング（`#/カテゴリ/セクション`）でトップのカテゴリカード → 詳細ページ（左サイドバー + コンテンツの 2 カラム）へ遷移。ページ内はフローティングアンカーメニュー（スクロール連動）で移動できる

## 構成

| ルート | 内容 |
|---|---|
| `#/` | トップ。カテゴリカード一覧 |
| `#/foundations` | カラー / タイポグラフィ / スペーシング / 角丸 / エレベーション |
| `#/components` | ボタン / バッジ / カード / Aside / コード / テーブル / タブ / リスト |
| `#/patterns` | ページヘッダ / ナビゲーション / 2 カラムレイアウト / フローティングメニュー / フッター |
| `#/guidelines` | 原則 / 使い方 / Do・Don't / バージョニング |

## 使い方

1. `designsystem.html` をブラウザで開く（`open designsystem.html`）
2. 新しい成果物を作るときは `:root` のトークン一式と dark の 2 ブロック（`@media (prefers-color-scheme: dark)` / `[data-theme="dark"]`）を**セットで**コピーする
3. 必要なコンポーネントの CSS + HTML を持っていく

詳細は `#/guidelines` を参照。

## バージョニング

semver で管理する。トークンの値変更は minor、トークン名の変更・削除は major（成果物側が名前で参照しているため breaking）。
