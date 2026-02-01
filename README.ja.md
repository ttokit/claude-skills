# claude-skills

Claude Code 用のスキルコレクション。PRレビュー対応などの生産性向上スキルを提供します。

[English README](README.md)

## 前提条件

- [GitHub CLI](https://cli.github.com/) (`gh` コマンド)
- [gh-pr-review](https://github.com/agynio/gh-pr-review) 拡張機能:
  ```bash
  gh extension install agynio/gh-pr-review
  ```

## インストール

```bash
npx skills add ttokit/claude-skills
```

pr-review-response のみをインストールする場合:
```bash
npx skills add ttokit/claude-skills --skill pr-review-response
```

## 含まれるスキル

### pr-review-response

PRレビュー指摘への対応を支援するスキル。

**機能:**
- レビューコメントの取得と妥当性分析
- 対応方針の確認（修正/反論/スキップ）
- コード修正の実行とコミット作成
- レビューコメントへの返信

**使い方:**
1. 対応したいPRのブランチにチェックアウト
2. Claude Code で `/pr-review-response` を実行
3. 対話形式のプロンプトに従って分析・確認・対応

または「レビュー対応」「PR指摘に対応」と入力してトリガー。

**ワークフロー:**
- 3フェーズプロセス: 分析 → 確認 → 実行
- 並列サブエージェントで複数コメントを効率的に分析
- 現在のブランチからPRを自動特定（PR番号指定不要）

**特長:**
- **エンドツーエンド自動化** - コード修正 → テスト → フォーマット → コミット → プッシュ → 返信を一気通貫
- **妥当性のスマート評価** - 各コメントをvalid/invalid/partialの3段階で評価し根拠を提示
- **対応前に必ずユーザー確認** - 各コメントへの対応方針はAskUserQuestionで確認してから実行
- **返信テンプレート** - 4種類のテンプレート（修正完了/反論/部分同意/質問説明）でプロフェッショナルな返信
- **未解決のインラインコメントのみ取得** - インライン以外のコメント（PR全体へのコメント等）は明示的に指示が必要
- **コメントを自動resolveしない** - ユーザーがPR上で確認・編集可能な状態を維持

**トリガー:**
- "review response", "PR comments", "address feedback"
- "respond to review", "PR #123 review"
- 「レビュー対応」「PR指摘」「コメント対応」

**言語サポート:**
- PR言語を自動検出（英語/日本語）
- 検出した言語で返信
- "reply in [language]" でオーバーライド可能

**デモ:**
- [Demo PR (English)](https://github.com/ttokit/claude-skills/pull/1) - 英語でレビュー返信
- [Demo PR (日本語)](https://github.com/ttokit/claude-skills/pull/2) - 日本語でレビュー返信

### skill-optimizer

```bash
npx skills add ttokit/claude-skills --skill skill-optimizer
```

Anthropic公式ベストプラクティスに基づいてスキルを分析・最適化するスキル。

**機能:**
- 品質スコア算出（A/B/C/D/Fグレード）
- 構造分析（Progressive Disclosure準拠）
- descriptionフィールドの改善提案
- エラーハンドリングと例の追加推奨
- MCP統合ガイダンス（該当する場合）

**使い方:**
1. `/skill-optimizer` または `/skill-optimizer [skill-path]` を実行
2. 分析対象のスキルを選択（パス未指定の場合）
3. 分析レポートと品質スコアを確認
4. 適用する改善カテゴリを選択
5. 元のファイルに変更が適用される（git復元可能）

**トリガー:**
- "improve skill", "optimize skill", "review skill"
- "check SKILL.md", "follow best practices"
- 「スキルを改善」「最適化して」「ベストプラクティスに従う」

**分析カテゴリ:**
| カテゴリ | 配点 | 観点 |
|----------|------|------|
| Frontmatter | 20 | YAML構文、必須フィールド、セキュリティ |
| Description | 25 | WHAT + WHEN、トリガーフレーズ、具体性 |
| Structure | 20 | Progressive Disclosure、ファイルサイズ |
| Content | 20 | エラーハンドリング、例、明確さ |
| Additional | 15 | references/活用、MCP統合 |

**参照ドキュメント:**
- YAMLフロントマター仕様
- Description記述ベストプラクティス
- Progressive Disclosure設計ガイド
- 5つのワークフローパターン（Sequential、Multi-MCP、Iterative、Context-aware、Domain-specific）
- よくある問題のトラブルシューティング
- 品質チェックリスト

## ライセンス

MIT
