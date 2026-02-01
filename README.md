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

```bash
npx skills add ttokit/claude-skills
```

Or install only pr-review-response:
```bash
npx skills add ttokit/claude-skills --skill pr-review-response
```

## Included Skills

### pr-review-response

Assists with responding to PR review comments.

**Features:**
- Fetch review comments and analyze validity
- Confirm response approach (fix/disagree/skip)
- Execute code changes and create commits
- Reply to review comments

**Usage:**
1. Checkout the branch for the PR you want to handle
2. Run `/pr-review-response` in Claude Code
3. Follow the interactive prompts to analyze, confirm, and respond to comments

Or trigger by typing "respond to review" or "address PR feedback".

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

**Demo:**
- [Demo PR (English)](https://github.com/ttokit/claude-skills/pull/1) - English review response
- [Demo PR (日本語)](https://github.com/ttokit/claude-skills/pull/2) - Japanese review response

### skill-optimizer

```bash
npx skills add ttokit/claude-skills --skill skill-optimizer
```

Analyzes and optimizes existing skills based on official Anthropic best practices.

**Features:**
- Quality score calculation (A/B/C/D/F grades)
- Structural analysis (Progressive Disclosure adherence)
- Description field improvement suggestions
- Error handling and example recommendations
- MCP integration guidance (if applicable)

**Usage:**
1. Run `/skill-optimizer` or `/skill-optimizer [skill-path]`
2. Select a skill to analyze (if no path specified)
3. Review the analysis report and quality score
4. Select improvement categories to apply
5. Changes are applied to original files (git recovery available)

**Triggers:**
- "improve skill", "optimize skill", "review skill"
- "check SKILL.md", "follow best practices"
- "スキルを改善", "最適化して"

**Analysis Categories:**
| Category | Points | Focus |
|----------|--------|-------|
| Frontmatter | 20 | YAML syntax, required fields, security |
| Description | 25 | WHAT + WHEN, trigger phrases, specificity |
| Structure | 20 | Progressive Disclosure, file size |
| Content | 20 | Error handling, examples, clarity |
| Additional | 15 | references/ usage, MCP integration |

**Reference Documentation:**
- YAML frontmatter specification
- Description writing best practices
- Progressive Disclosure design guide
- 5 workflow patterns (Sequential, Multi-MCP, Iterative, Context-aware, Domain-specific)
- Troubleshooting common issues
- Quality checklist

## License

MIT
