# gh-pr-review Command Reference

GitHub CLI extension optimized for AI agent PR review comment operations.

## Installation

```bash
gh extension install agynio/gh-pr-review
gh extension upgrade agynio/gh-pr-review  # Upgrade
```

## Main Commands

### View Review Comments

```bash
# Basic (all comments)
gh pr-review review view --pr {number}

# Unresolved only
gh pr-review review view --pr {number} --unresolved

# JSON output (for AI processing)
gh pr-review review view --pr {number} --unresolved --json

# With repository specification
gh pr-review review view -R owner/repo --pr {number}
```

**Main Filters**:
| Flag | Description |
|------|-------------|
| `--unresolved` | Unresolved threads only |
| `--reviewer <login>` | Filter by specific reviewer |
| `--states <list>` | Filter by review state (APPROVED, etc.) |
| `--not_outdated` | Exclude outdated threads |
| `--include-comment-node-id` | Include GraphQL node ID |

### Reply to Threads

```bash
gh pr-review comments reply \
  --repo {owner/repo} \
  --pr {pr_number} \
  --thread-id {thread_id} \
  --body "Reply content"
```

**Note**: `--repo` and `--pr` are required options. Omitting them causes an error.

`thread_id` is a GraphQL node ID starting with `PRRT_`.

**Important**: Comment ID (`PRRC_...`) and thread ID (`PRRT_...`) are different. REST API only returns comment IDs. Thread IDs must be fetched via GraphQL API:

```bash
gh api graphql -f query='
{
  repository(owner: "OWNER", name: "REPO") {
    pullRequest(number: PR_NUMBER) {
      reviewThreads(first: 50) {
        nodes {
          id          # ← This is the thread_id (PRRT_...)
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

### Resolve Threads

```bash
gh pr-review threads resolve --thread-id {thread_id}
```

### Pending Review Operations

```bash
# Start (get review ID)
gh pr-review review --start -R owner/repo {pr_number}

# Add inline comment
gh pr-review review --add-comment \
  --review-id PRR_... \
  --path path/to/file.ts \
  --line 42 \
  --body "Comment content"

# Submit
gh pr-review review --submit \
  --review-id PRR_... \
  --event APPROVE  # APPROVE, REQUEST_CHANGES, COMMENT
```

## JSON Output Schema

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
        "body": "Comment body",
        "created_at": "2025-01-01T00:00:00Z"
      }]
    }]
  }]
}
```

## Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| `extension not found` | Not installed | `gh extension install agynio/gh-pr-review` |
| `not found` | PR/thread doesn't exist | Verify PR number/thread_id |
| `permission denied` | Insufficient permissions | Check repository access |
| `must specify a pull request via --pr or selector` | `--pr` not specified | Specify both `--repo` and `--pr` |

## Fallback with Standard gh api

If gh-pr-review is unavailable:

```bash
# Fetch comments
gh api repos/{owner}/{repo}/pulls/{pr}/comments

# Reply
gh api repos/{owner}/{repo}/pulls/{pr}/comments/{comment_id}/replies \
  -f body="Reply content"

# PR comment
gh pr comment {pr} --body "Comment content"
```
