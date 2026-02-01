# Progressive Disclosure Design Guide

Based on "The Complete Guide to Building Skills for Claude" (Chapter 1, p.5)

## The Three-Level System

Skills use a three-level system to minimize token usage while maintaining specialized expertise:

### First Level (YAML frontmatter)

**Always loaded** in Claude's system prompt.

Provides just enough information for Claude to know when each skill should be used without loading all of it into context.

**Content:**
- name
- description (with trigger phrases)
- metadata

### Second Level (SKILL.md body)

**Loaded when Claude thinks the skill is relevant** to the current task.

Contains the full instructions and guidance.

**Content:**
- Core workflow steps
- Main instructions
- Quick examples
- Links to references

### Third Level (Linked files)

**Additional files bundled within the skill directory** that Claude can choose to navigate and discover only as needed.

**Content:**
- Detailed documentation (references/)
- API guides
- Extended examples
- Troubleshooting guides

## Why This Matters

This progressive disclosure minimizes token usage while maintaining specialized expertise.

**Without progressive disclosure:**
- All skill content loaded every time
- High token consumption
- Slower responses
- Context window pollution

**With progressive disclosure:**
- Only relevant content loaded
- Efficient token usage
- Faster responses
- Clean context

## SKILL.md Optimization Guidelines

### Keep SKILL.md Under 5000 Words

If your SKILL.md exceeds this:
1. Move detailed docs to `references/`
2. Link to references instead of inline content
3. Keep only core instructions in SKILL.md

### What Belongs in SKILL.md

- Core workflow (numbered steps)
- Critical instructions
- Quick examples (1-2 per use case)
- Links to detailed references

### What Belongs in references/

- API documentation
- Extended examples
- Troubleshooting guides
- Configuration details
- Edge case handling
- Template collections

## Criteria for Splitting to references/

Move content to references/ when:

| Criterion | Threshold |
|-----------|-----------|
| Section length | > 500 words |
| Used only in specific scenarios | Yes |
| Reference material (not instructions) | Yes |
| Detailed API/CLI documentation | Yes |
| Troubleshooting guides | Yes |
| Extended examples | > 3 examples |

## File Structure Example

```
your-skill-name/
├── SKILL.md              # Core instructions only
└── references/
    ├── api-guide.md      # Detailed API documentation
    ├── examples.md       # Extended examples
    └── troubleshooting.md # Common issues & solutions
```

## Linking to References

In SKILL.md, reference files clearly:

```markdown
## API Usage

For detailed API patterns, see `./references/api-guide.md`.

## Troubleshooting

If you encounter issues, consult `./references/troubleshooting.md`.
```

## Checklist

- [ ] SKILL.md under 5000 words
- [ ] Core workflow at the top of SKILL.md
- [ ] Detailed documentation moved to references/
- [ ] References clearly linked from SKILL.md
- [ ] Each reference file has a focused purpose
- [ ] Critical instructions not buried in references
