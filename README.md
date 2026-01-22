# claude-skills

Skill collection for Claude Code. Provides productivity-enhancing skills including PR review response.

[日本語版 README はこちら](README.ja.md)

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

**Key Benefits:**
- **Fetches only unresolved inline comments** - Non-inline comments (e.g., PR-level comments) require explicit instruction
- **Confirms with user before each action** - Uses AskUserQuestion to confirm approach for each comment
- **Never auto-resolves comments** - Keeps comments visible on PR for user verification and editing

**Triggers:**
- "review response", "PR comments", "address feedback"
- "respond to review", "PR #123 review"

**Language Support:**
- Automatically detects PR language (English/Japanese)
- Replies in detected language
- Override with "reply in [language]"

**Prerequisite:**
```bash
gh extension install agynio/gh-pr-review
```

## License

MIT
