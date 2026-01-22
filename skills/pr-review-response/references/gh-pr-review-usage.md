# gh-pr-review コマンドリファレンス

GitHub CLI拡張機能。AIエージェント向けに最適化されたPRレビューコメント操作ツール。

## インストール

```bash
gh extension install agynio/gh-pr-review
gh extension upgrade agynio/gh-pr-review  # アップグレード
```

## 主要コマンド

### レビューコメント表示

```bash
# 基本（全コメント）
gh pr-review review view --pr {number}

# 未解決のみ
gh pr-review review view --pr {number} --unresolved

# JSON出力（AI処理向け）
gh pr-review review view --pr {number} --unresolved --json

# リポジトリ指定
gh pr-review review view -R owner/repo --pr {number}
```

**主要フィルタ**:
| フラグ | 説明 |
|--------|------|
| `--unresolved` | 未解決スレッドのみ |
| `--reviewer <login>` | 特定レビュアーに限定 |
| `--states <list>` | レビュー状態でフィルタ（APPROVED等） |
| `--not_outdated` | 古いスレッドを除外 |
| `--include-comment-node-id` | GraphQLノードID含める |

### スレッドへの返信

```bash
gh pr-review comments reply \
  --repo {owner/repo} \
  --pr {pr_number} \
  --thread-id {thread_id} \
  --body "返信内容"
```

**注意**: `--repo` と `--pr` は必須オプション。省略するとエラーになる。

`thread_id`は`PRRT_`で始まるGraphQLノードID。

**重要**: コメントID（`PRRC_...`）とスレッドID（`PRRT_...`）は異なる。REST APIで取得できるのはコメントIDのみ。スレッドIDはGraphQL APIで取得する必要がある：

```bash
gh api graphql -f query='
{
  repository(owner: "OWNER", name: "REPO") {
    pullRequest(number: PR_NUMBER) {
      reviewThreads(first: 50) {
        nodes {
          id          # ← これがthread_id (PRRT_...)
          isResolved
          comments(first: 1) {
            nodes { body path }
          }
        }
      }
    }
  }
}'
```

### スレッド解決

```bash
gh pr-review threads resolve --thread-id {thread_id}
```

### ペンディングレビュー操作

```bash
# 開始（レビューID取得）
gh pr-review review --start -R owner/repo {pr_number}

# インラインコメント追加
gh pr-review review --add-comment \
  --review-id PRR_... \
  --path path/to/file.ts \
  --line 42 \
  --body "コメント内容"

# 提出
gh pr-review review --submit \
  --review-id PRR_... \
  --event APPROVE  # APPROVE, REQUEST_CHANGES, COMMENT
```

## JSON出力スキーマ

```json
{
  "reviews": [{
    "id": "PRR_...",
    "state": "APPROVED|CHANGES_REQUESTED|COMMENTED|PENDING",
    "author_login": "reviewer-name",
    "comments": [{
      "thread_id": "PRRT_...",
      "path": "src/file.ts",
      "line": 42,
      "is_resolved": false,
      "thread": [{
        "author_login": "reviewer-name",
        "body": "コメント本文",
        "created_at": "2025-01-01T00:00:00Z"
      }]
    }]
  }]
}
```

## エラーハンドリング

| エラー | 原因 | 対処 |
|--------|------|------|
| `extension not found` | 未インストール | `gh extension install agynio/gh-pr-review` |
| `not found` | PR/スレッドが存在しない | PR番号/thread_idを確認 |
| `permission denied` | 権限不足 | リポジトリアクセス権を確認 |
| `must specify a pull request via --pr or selector` | `--pr` 未指定 | `--repo` と `--pr` を指定 |

## 標準gh apiでのフォールバック

gh-pr-reviewが使用できない場合：

```bash
# コメント取得
gh api repos/{owner}/{repo}/pulls/{pr}/comments

# 返信
gh api repos/{owner}/{repo}/pulls/{pr}/comments/{comment_id}/replies \
  -f body="返信内容"

# PRコメント
gh pr comment {pr} --body "コメント内容"
```
