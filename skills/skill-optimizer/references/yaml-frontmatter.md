# YAML Frontmatter Reference

Based on "The Complete Guide to Building Skills for Claude" (Chapter 2, p.10-11; Reference B, p.31)

## Required Fields

```yaml
---
name: skill-name-in-kebab-case
description: What it does and when to use it. Include specific trigger phrases.
---
```

That's all you need to start.

## Field Requirements

### name (required)

- kebab-case only
- No spaces or capitals
- Should match folder name

**Correct:**
```yaml
name: my-cool-skill
```

**Wrong:**
```yaml
name: My Cool Skill
```

### description (required)

- **MUST include BOTH:**
  - What the skill does
  - When to use it (trigger conditions)
- Under 1024 characters
- No XML tags (< or >)
- Include specific tasks users might say
- Mention file types if relevant

## All Optional Fields

```yaml
name: skill-name
description: [required description]
license: MIT # Optional: License for open-source
allowed-tools: "Bash(python:*) Bash(npm:*) WebFetch" # Optional: Restrict tool access
metadata: # Optional: Custom fields
  author: Company Name
  version: 1.0.0
  mcp-server: server-name
  category: productivity
  tags: [project-management, automation]
  documentation: https://example.com/docs
  support: support@example.com
```

### license (optional)

- Use if making skill open source
- Common: MIT, Apache-2.0

### compatibility (optional)

- 1-500 characters
- Indicates environment requirements: e.g. intended product, required system packages, network access needs, etc.

### metadata (optional)

- Any custom key-value pairs
- Suggested: author, version, mcp-server

## Security Restrictions

### Forbidden in frontmatter:

- **XML angle brackets (< >)** - security restriction
- **Code execution in YAML** - uses safe YAML parsing
- **Skills with "claude" or "anthropic" in name** - reserved

**Why:** Frontmatter appears in Claude's system prompt. Malicious content could inject instructions.

## Common YAML Mistakes

```yaml
# Wrong - missing delimiters
name: my-skill
description: Does things

# Wrong - unclosed quotes
name: my-skill
description: "Does things

# Correct
---
name: my-skill
description: Does things
---
```

## Validation Checklist

- [ ] `---` delimiters at start and end of frontmatter
- [ ] `name` is kebab-case (lowercase, hyphens only)
- [ ] `name` matches folder name
- [ ] `description` includes WHAT and WHEN
- [ ] `description` under 1024 characters
- [ ] No XML tags anywhere in frontmatter
- [ ] No "claude" or "anthropic" prefix in name
- [ ] Valid YAML syntax (proper indentation, quotes)
