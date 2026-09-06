# AI Agent Pipeline Template

設計ドキュメント起点でAIエージェント(Claude Code等。Devinのような別の実装エージェントに
差し替えることも可能)を制御するためのテンプレートリポジトリです。技術スタックは
サービスごとに変わりますが、「AIエージェントの制御プロセス」自体は使い回せる、
という前提で構成しています。開発フローそのものの全体像(design doc PR→実装PR→
モデル更新PR、の単方向3本構成)は [`OVERVIEW.md`](./OVERVIEW.md) を参照。
(このREADMEの①②③④は下記「4層」の番号で、OVERVIEW.mdのPR番号とは別体系です)

## 構成の考え方(4層)

| 層 | 役割 | このリポジトリでの実体 |
|---|---|---|
| ① 指示 | 前提知識のマウント(方針) | `AGENTS.md` / `CLAUDE.md` |
| ② 制御 | 常時効くガードレール(強制) | `.claude/settings.json` |
| ③ フック | セッション内の決定論的処理(任意・今は最小限) | `.claude/settings.json` の hooks キー |
| ④ 分業 | 知識の切り出し・独立視点での作業 | `.claude/skills/` / `.claude/agents/` |

判断基準の要約:

- **コーディング指示の起点は設計ドキュメントの「受け入れ条件」**。ドメインモデルは
  安定した語彙であり、今回のタスク粒度の指示にはならない。曖昧な形容詞ではなく
  「入力Xのとき出力Yになる」という検証可能な形で書く(`docs/design/0000-template.md`)。
- **①には**「AIの解釈に任せていい方針」を書く。長くなりすぎたら④に逃がす。
- **②には**「AIの解釈に任せると危ういルール」を書く。文章ではなく機械的な allow/deny。
- **④のSkill**は「知識・手順」。同じ文脈の中で読み込ませる。
- **④のSubagent**は「独立した視点」。特に自己採点バイアスを避けたいレビュー・検証に使う。
  レビュー用Subagentには作成時の会話履歴を渡さず、成果物だけを渡すこと。

## 不可逆な意思決定には人を挟む

コード実装はレビュー・テスト・revertで機械的に取り消せるが、設計ドキュメント・ADR・
ドメインモデルはその後の思考の前提になるため、取り消しコストが非対称に高い。
このテンプレートでは「コード実装以外(設計ドキュメント・ADR・ドメインモデル更新)は
人が承認する」ことを前提にパイプラインを組んでいる。マージ判断のリスクベース自動化は、
最初から作り込まず、運用ログが溜まってから段階的に導入する。

## ディレクトリ構成

```
.
├── OVERVIEW.md                # 開発フロー全体像(①design doc PR→②実装PR→③モデル更新PR)
├── SETUP.md                    # テンプレート使用後のチェックリスト
├── AGENTS.md                 # 唯一の正。方針・スタック・禁止事項
├── CLAUDE.md                 # AGENTS.mdをインポートするだけの薄いファイル
├── .claude/
│   ├── settings.json          # パーミッション(allow/deny)とhooksの器
│   ├── agents/                # Subagent定義(独立視点のレビュー・検証役)
│   │   ├── reviewer.md
│   │   └── verifier.md
│   └── skills/                # 手順・ノウハウ(同一文脈で読み込む知識)
│       └── example-skill/
│           └── SKILL.md
├── docs/
│   ├── design/                 # 設計ドキュメント(コーディング指示の起点。受け入れ条件が必須項目)
│   │   ├── README.md
│   │   └── 0000-template.md
│   ├── domain-model.md        # ドメインモデル(更新は人が承認)
│   └── adr/                   # 意思決定履歴(人の決定そのもの)
│       ├── README.md
│       └── 0000-template.md
└── .github/workflows/
    ├── design-doc-pipeline.yml   # ①design doc PRマージ→②実装PR作成+レビュー
    └── model-update-pipeline.yml # ②実装PRマージ→③モデル更新PR提案
```

## 使い始め方

"Use this template" でリポジトリを作成したら、まず [`SETUP.md`](./SETUP.md) の
チェックリストを実行すること。ラベル・ブランチ保護・Secretsなど、
GitHubのTemplate repository機能では複製されない設定が含まれるため。
