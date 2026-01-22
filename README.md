# claude-skills

Skill collection for Claude Code. Provides productivity-enhancing skills including PR review response.

[日本語版 README はこちら](README.ja.md)

## Prerequisites

- [GitHub CLI](https://cli.github.com/) (`gh` command)
- [gh-pr-review](https://github.com/agynio/gh-pr-review) extension:
  ```bash
  gh extension install agynio/gh-pr-review
  ```

## Installation

### Option 1: Inside Claude Code (Recommended)

**Step 1: Add the marketplace**
```
/plugin marketplace add ttokit/claude-skills
```

**Step 2: Install the plugin**
```
/plugin install pr-review-response@ttokit-claude-skills
```

### Option 2: Shell Command

```bash
claude plugin install pr-review-response@ttokit-claude-skills
```

With project scope (for team sharing):
```bash
claude plugin install pr-review-response@ttokit-claude-skills --scope project
```

## Included Skills

### pr-review-response

Assists with responding to PR review comments.

**Features:**
- Fetch review comments and analyze validity
- Confirm response approach (fix/disagree/skip)
- Execute code changes and create commits
- Reply to review comments

**Workflow:**
- 3-phase process: Analyze → Confirm → Execute
- Parallel sub-agent analysis for multiple comments
- Auto-detects PR from current branch (no PR number needed)

**Key Benefits:**
- **End-to-end automation** - Code fix → Test → Format → Commit → Push → Reply in one flow
- **Smart validity assessment** - Evaluates each comment as valid/invalid/partial with reasoning
- **Confirms with user before each action** - Uses AskUserQuestion to confirm approach for each comment
- **Response templates** - 4 templates (fix completed, disagreement, partial agreement, clarification) for consistent, professional replies
- **Fetches only unresolved inline comments** - Non-inline comments (e.g., PR-level comments) require explicit instruction
- **Never auto-resolves comments** - Keeps comments visible on PR for user verification and editing

**Triggers:**
- "review response", "PR comments", "address feedback"
- "respond to review", "PR #123 review"

**Language Support:**
- Automatically detects PR language (English/Japanese)
- Replies in detected language
- Override with "reply in [language]"

## License

MIT
