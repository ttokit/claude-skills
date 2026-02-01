# Description Field Best Practices

Based on "The Complete Guide to Building Skills for Claude" (Chapter 2, p.11-12)

## Why Description Matters

According to Anthropic's engineering blog: "This metadata...provides just enough information for Claude to know when each skill should be used without loading all of it into context." This is the first level of progressive disclosure.

## Structure

```
[What it does] + [When to use it] + [Key capabilities]
```

## Examples of Good Descriptions

```yaml
# Good - specific and actionable
description: Analyzes Figma design files and generates developer handoff documentation. Use when user uploads .fig files, asks for "design specs", "component documentation", or "design-to-code handoff".

# Good - includes trigger phrases
description: Manages Linear project workflows including sprint planning, task creation, and status tracking. Use when user mentions "sprint", "Linear tasks", "project planning", or asks to "create tickets".

# Good - clear value proposition
description: End-to-end customer onboarding workflow for PayFlow. Handles account creation, payment setup, and subscription management. Use when user says "onboard new customer", "set up subscription", or "create PayFlow account".
```

## Examples of Bad Descriptions

```yaml
# Too vague
description: Helps with projects.

# Missing triggers
description: Creates sophisticated multi-page documentation systems.

# Too technical, no user triggers
description: Implements the Project entity model with hierarchical relationships.
```

## Key Elements

### 1. WHAT - What the skill does

Be specific about the outcome:
- "Analyzes Figma design files and generates developer handoff documentation"
- "Manages Linear project workflows including sprint planning"
- "End-to-end customer onboarding workflow for PayFlow"

### 2. WHEN - Trigger conditions

Include specific phrases users would say:
- "Use when user uploads .fig files"
- "Use when user mentions 'sprint', 'Linear tasks'"
- "Use when user says 'onboard new customer'"

### 3. Key Capabilities (optional but helpful)

Mention core features:
- "Handles account creation, payment setup, and subscription management"
- "Including sprint planning, task creation, and status tracking"

## Character Limit

- Maximum: **1024 characters**
- Keep it concise but comprehensive

## Trigger Phrase Tips

### Do include:
- Specific task verbs ("create", "analyze", "set up")
- Product/service names ("Linear", "PayFlow", "Figma")
- File types if relevant (".fig files", "CSV files", "PDF documents")
- Common user phrasings ("help me plan", "I need to")

### Avoid:
- Generic terms alone ("project", "data", "files")
- Technical jargon users wouldn't say
- Overly broad triggers that cause over-triggering

## Debugging Tip

Ask Claude: "When would you use the [skill name] skill?" Claude will quote the description back. Adjust based on what's missing.

## Checklist

- [ ] Description includes WHAT the skill does
- [ ] Description includes WHEN to use it (trigger conditions)
- [ ] Under 1024 characters
- [ ] Contains 3+ specific trigger phrases
- [ ] Not too vague or generic
- [ ] Mentions file types if applicable
- [ ] Uses natural user language, not technical jargon
