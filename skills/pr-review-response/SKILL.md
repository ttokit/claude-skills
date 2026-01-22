---
name: pr-review-response
description: |
  PRレビュー指摘への対応を支援するスキル。レビューコメントの妥当性分析、
  対応方針の確認、コード修正実行、コメント返信を行う。
  トリガー：「レビュー対応」「PR指摘」「review response」「コメント対応」
  「レビューコメント」「指摘対応」「PR #123 のレビュー対応」
---

# PR Review Response

PRレビュー指摘に対して、分析→確認→実行の3フェーズで効率的に対応する。

## 前提条件

gh-pr-review拡張機能が必要。未インストールの場合：
```bash
gh extension install agynio/gh-pr-review
```

## PR特定

1. PR番号が指定されている場合はそれを使用
2. 指定がなければ現在のブランチからPRを特定：
```bash
# リポジトリ情報を取得
gh repo view --json nameWithOwner -q '.nameWithOwner'
# PR番号を取得
gh pr view --json number -q '.number'
```

## Phase 1: コメント取得・分析

### 未解決コメントの取得

```bash
gh pr-review review view --repo {owner/repo} --pr {pr_number} --unresolved
```

詳細なオプションは [gh-pr-review-usage.md](references/gh-pr-review-usage.md) を参照。

### 分析（並列サブエージェント）

各コメントに対してサブエージェント（Task tool, subagent_type=general-purpose）を起動し、以下を分析：

- **妥当性判定**: valid / invalid / partial
- **推奨アクション**: fix / reply / ignore
- **修正案**: 修正が必要な場合の具体的な変更内容

サブエージェントへのプロンプト例：
```
以下のPRレビューコメントを分析してください。

コメント:
{comment_body}

対象ファイル: {path}
対象行: {line}

分析結果として以下を返却してください：
1. 妥当性（valid/invalid/partial）と理由
2. 推奨アクション（fix/reply/ignore）
3. 修正が必要な場合の具体的な修正案
```

## Phase 2: ユーザー確認

**重要**: この処理は親プロセスで実行する。サブエージェント内ではAskUserQuestionが使用できない。

各コメントごとに分析結果を表示し、AskUserQuestionで確認：

```
## コメント {n}/{total}

**ファイル**: {path}:{line}
**コメント**: {comment_body}
**分析結果**: {validity} - {reason}
**推奨**: {recommended_action}

どのように対応しますか？
- 修正する
- 指摘に反論
- スキップ
```

## Phase 3: 実行

承認された対応ごとにサブエージェント（Task tool）を起動。

### 修正する場合

1. コード修正を実行
2. テスト実行: `pnpm test`
3. フォーマット: `pnpm fmt`
4. コミット作成（Conventional Commits形式）
5. リモートにプッシュ
6. コメント返信（コミットハッシュとURLを含める）

### 反論する場合

理由を含めてコメント返信のみ

## コメント返信

### インラインコメントへの返信

```bash
gh pr-review comments reply \
  --repo {owner/repo} \
  --pr {pr_number} \
  --thread-id {thread_id} \
  --body "返信内容"
```

**注意**: `--repo` と `--pr` は必須オプション。省略すると `must specify a pull request via --pr or selector` エラーが発生する。

### PRレベルのコメントへの返信

```bash
gh pr comment {pr_number} --body "> {引用}

返信内容"
```

## 返信テンプレート

### 修正完了

```
コミット {short_hash} で修正しました。

変更内容：
- {変更点}
```

### 反論

```
ご指摘ありがとうございます。

以下の理由により、現状の実装を維持します：
- {理由}

ご確認いただけますでしょうか。
```
