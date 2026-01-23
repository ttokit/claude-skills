---
name: pr-review-response
description: "Handles PR review comments through analyze-confirm-execute workflow. Analyzes comment validity, confirms response approach with user, executes code changes, and replies to reviewers. Use when responding to PR review comments, addressing feedback, or handling PR reviews."
allowed-tools: ["Bash", "Glob", "Grep", "Read", "Edit", "Write", "Task", "AskUserQuestion"]
---

# PR Review Response

Efficiently handle PR review comments through a 3-phase process: Analyze → Confirm → Execute.

## Prerequisites

The gh-pr-review extension is required. If not installed:
```bash
gh extension install agynio/gh-pr-review
```

## PR Identification

1. Use the PR number if specified
2. Otherwise, identify PR from current branch:
```bash
# Get repository info
gh repo view --json nameWithOwner -q '.nameWithOwner'
# Get PR number
gh pr view --json number -q '.number'
```

## Phase 1: Fetch & Analyze Comments

### Language Detection

At the start of Phase 1, detect the primary language for comment replies:

1. Fetch PR metadata:
   ```bash
   gh pr view {pr_number} --json title,body
   ```

2. Analyze PR title and body:
   - Japanese characters (hiragana/katakana/kanji): **Japanese**
   - English/Latin characters: **English**
   - Mixed/unclear: **Ask user via AskUserQuestion**
   - Empty/symbols only/no text content: **Default to English**, then ask user to confirm

3. Display result:
   ```
   Detected reply language: {detected_language}
   (Override with "reply in [language]" at any time)
   ```

4. Load appropriate template from `./templates/{lang}.md` (relative to skill directory)

### Fetch Unresolved Comments

```bash
gh pr-review review view -R owner/repo --pr {pr_number} --unresolved
```

See [gh-pr-review-usage.md](./reference/gh-pr-review-usage.md) for detailed options.

### Analysis (Parallel Sub-agents)

Launch a sub-agent (Task tool, subagent_type=general-purpose) for each comment to analyze:

- **Validity assessment**: valid / invalid / partial
- **Recommended action**: fix / reply / ignore
- **Fix proposal**: Specific changes if fix is needed

Sub-agent prompt example:
```
Analyze the following PR review comment.

Comment:
{comment_body}

Target file: {path}
Target line: {line}

Return the following analysis:
1. Validity (valid/invalid/partial) with reasoning
2. Recommended action (fix/reply/ignore)
3. Specific fix proposal if changes are needed
```

## Phase 2: User Confirmation

**Important**: This process must run in the parent process. AskUserQuestion cannot be used within sub-agents.

Display analysis results for each comment and confirm with AskUserQuestion:

```
## Comment {n}/{total}

**File**: {path}:{line}
**Comment**: {comment_body}
**Analysis**: {validity} - {reason}
**Recommendation**: {recommended_action}

How would you like to respond?
- Fix the issue
- Disagree with feedback
- Skip
```

## Phase 3: Execution

Launch a sub-agent (Task tool) for each approved action.

### When Fixing

1. Execute code changes
2. Run tests: `pnpm test`
3. Format code: `pnpm fmt`
4. Create commit (Conventional Commits format)
5. Push to remote
6. Reply to comment (include commit hash and URL)

### When Disagreeing

Reply to comment with reasoning only

## Comment Replies

### Replying to Inline Comments

```bash
gh pr-review comments reply \
  --repo {owner/repo} \
  --pr {pr_number} \
  --thread-id {thread_id} \
  --body "Reply content"
```

**Note**: `--repo` and `--pr` are required options. Omitting them causes `must specify a pull request via --pr or selector` error.

### Replying to PR-level Comments

```bash
gh pr comment {pr_number} --body "> {quote}

Reply content"
```

## Response Templates

Use templates from `templates/` based on detected language:
- English: `./templates/en.md`
- Japanese: `./templates/ja.md`

## Internal Processing Language Rules

| Context | Language |
|---------|----------|
| Sub-agent prompts | English + language instruction |
| Commit messages | Always English (Conventional Commits) |
| Error messages | User's CLAUDE.md setting |
| AskUserQuestion UI | User's CLAUDE.md setting |
| Comment replies | Detected language (user-overridable) |
