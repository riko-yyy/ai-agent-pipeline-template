# SETUP.md

このリポジトリを "Use this template" で作成した直後にやること。
GitHubのTemplate repository機能はファイルは複製するが、
以下の周辺設定は複製されないため手動(または初回のみのスクリプト)で作る必要がある。

## チェックリスト

### ブランチ保護 / 必須レビュー(唯一の承認ゲート)
- [ ] `docs/design/**` を変更するPRに、人のレビュー承認を必須にする
      (このマージが `design-doc-pipeline.yml` の実装トリガーになる)
- [ ] `docs/adr/**` を変更するPRに、人のレビュー承認を必須にする
- [ ] `docs/domain-model.md` を変更するPRに、人のレビュー承認を必須にする
      (このブランチ保護が `model-update-pipeline.yml` が作るモデル更新PRの承認ゲートになる)

### Claude GitHub App / Secrets
- [ ] [github.com/apps/claude](https://github.com/apps/claude) からClaude GitHub Appをこのリポジトリにインストールする
      (Contents / Issues / Pull requests の read/write 権限が必要)
- [ ] `ANTHROPIC_API_KEY` をリポジトリのSecretsに登録する
      (ターミナルで `claude` → `/install-github-app` を実行すると上記2つを自動でやってくれる)

### 中身の書き換え
- [ ] `AGENTS.md` の `## Stack` 以下をこのサービスの技術スタックに合わせて埋める
- [ ] `.claude/settings.json` の `deny` に、このプロジェクト固有の機密パスを追加する
- [ ] `.claude/skills/example-skill/` を実際のスキルにリネーム・内容を書き換える(不要なら削除)
- [ ] `.claude/agents/reviewer.md` / `verifier.md` の観点をプロジェクトに合わせて調整する

### 運用開始後(すぐでなくてよい)
- [ ] しばらくは全PRを人がマージ判断し、PRの種類・サイズ・指摘の有無をログに残す
- [ ] ログが溜まったら、自動マージ可能なパターンをラベリングルールとして切り出す

## このチェックリストの位置づけ

テンプレートのファイル構成だけ複製して安心してしまうと、
「ブランチ保護が設定されておらず、design docが誰でもマージできてしまう」
といった状態に気づかないまま運用が始まりがちです。上から順に潰してから最初のPRを流してください。
