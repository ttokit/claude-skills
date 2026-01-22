# claude-skills

Claude Code 用のスキルコレクション。PRレビュー対応などの生産性向上スキルを提供します。

[English README](README.md)

## インストール

### オプション1: Claude Code 内から（推奨）

**ステップ1: マーケットプレースを追加**
```
/plugin marketplace add ttokit/claude-skills
```

**ステップ2: プラグインをインストール**
```
/plugin install pr-review-response@ttokit-claude-skills
```

### オプション2: シェルコマンド

```bash
claude plugin install pr-review-response@ttokit-claude-skills
```

プロジェクトスコープ（チーム共有用）:
```bash
claude plugin install pr-review-response@ttokit-claude-skills --scope project
```

## 含まれるスキル

### pr-review-response

PRレビュー指摘への対応を支援するスキル。

**機能:**
- レビューコメントの取得と妥当性分析
- 対応方針の確認（修正/反論/スキップ）
- コード修正の実行とコミット作成
- レビューコメントへの返信

**特長:**
- **未解決のインラインコメントのみ取得** - インライン以外のコメント（PR全体へのコメント等）は明示的に指示が必要
- **対応前に必ずユーザー確認** - 各コメントへの対応方針はAskUserQuestionで確認してから実行
- **コメントを自動resolveしない** - ユーザーがPR上で確認・編集可能な状態を維持

**トリガー:**
- "review response", "PR comments", "address feedback"
- "respond to review", "PR #123 review"
- 「レビュー対応」「PR指摘」「コメント対応」

**言語サポート:**
- PR言語を自動検出（英語/日本語）
- 検出した言語で返信
- "reply in [language]" でオーバーライド可能

**前提条件:**
```bash
gh extension install agynio/gh-pr-review
```

## ライセンス

MIT
