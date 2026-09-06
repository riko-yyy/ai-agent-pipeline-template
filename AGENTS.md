# AGENTS.md

このファイルがAIエージェント向け指示の唯一の正(single source of truth)。
CLAUDE.md 等ツール固有ファイルはこれをインポートするだけにする。

## Stack
<!-- サービスごとに書き換える -->
- 言語:
- フレームワーク:
- データフェッチ:
- 状態管理:
- テスト:
- Lint/Format:

## 禁止事項
<!-- AIの解釈に任せてよい「方針」。機械的に強制したいものは .claude/settings.json へ -->
- (例)新規に別の状態管理ライブラリを導入しない
- (例)マイグレーションを直接実行しない。マイグレーションファイルの生成まで

## コーディング規約
<!-- 命名規則・ディレクトリ構成・設計方針など -->

## よく使うコマンド
<!-- 毎回タイプさせなくていいコマンド一覧。.claude/settings.json の allow と対応させる -->
- build:
- test:
- lint:

## プロジェクト固有の文脈
<!-- ドメイン用語、なぜその設計にしたかの背景。詳細は docs/domain-model.md と docs/adr/ を参照 -->
- コーディング指示の起点: [docs/design/](./docs/design/)(受け入れ条件を必ず参照する)
- ドメインモデル: [docs/domain-model.md](./docs/domain-model.md)
- 意思決定履歴(ADR): [docs/adr/](./docs/adr/)

## 大きくなりすぎた手順・ノウハウの置き場
<!-- ここに書きたくなった長い手順書は .claude/skills/ に切り出す -->
- .claude/skills/ 配下を参照
