# claude-skills

Claude Code 用のスキルコレクション。PRレビュー対応などの生産性向上スキルを提供します。

## インストール

```bash
claude plugin install ttokit/claude-skills
```

## 含まれるスキル

### pr-review-response

PRレビュー指摘への対応を支援するスキル。

**機能:**
- レビューコメントの取得と妥当性分析
- 対応方針の確認（修正/反論/スキップ）
- コード修正の実行とコミット作成
- レビューコメントへの返信

**トリガー:**
- 「レビュー対応」「PR指摘」「review response」
- 「コメント対応」「レビューコメント」「指摘対応」
- 「PR #123 のレビュー対応」

**前提条件:**
```bash
gh extension install agynio/gh-pr-review
```

## ライセンス

MIT
