# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

Claude Code skill collection. Skills are installed via `npx skills add ttokit/claude-skills`.

## Prerequisites

```bash
# GitHub CLI (required)
brew install gh

# gh-pr-review extension (required for pr-review-response skill)
gh extension install agynio/gh-pr-review
```

## Architecture

Skills live in `skills/{skill-name}/` with this structure:

- `SKILL.md` - Skill specification with YAML frontmatter:
  ```yaml
  ---
  name: skill-name
  description: "When to trigger this skill"
  allowed-tools: ["Bash", "Read", "Edit", ...]
  ---
  ```
- `templates/` - Language-specific templates (en.md, ja.md)
- `reference/` - CLI documentation and usage guides

## Adding a New Skill

1. Create `skills/{skill-name}/SKILL.md` with frontmatter and workflow specification
2. Add `templates/` for language-specific content if needed
3. Add `reference/` for CLI/API documentation if needed
4. Document in README.md and README.ja.md

## Conventions

- Commit messages: English, Conventional Commits format (e.g., `feat(skill-name): add feature`)
