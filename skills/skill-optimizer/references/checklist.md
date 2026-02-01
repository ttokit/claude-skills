# Quality Checklist

Based on "The Complete Guide to Building Skills for Claude" (Reference A, p.30)

Use this checklist to validate your skill before and after upload. If you want a faster start, use the skill-creator skill to generate your first draft, then run through this list to make sure you haven't missed anything.

---

## Before You Start

- [ ] Identified 2-3 concrete use cases
- [ ] Tools identified (built-in or MCP)
- [ ] Reviewed this guide and example skills
- [ ] Planned folder structure

---

## During Development

### File Structure
- [ ] Folder named in kebab-case
- [ ] SKILL.md file exists (exact spelling)

### YAML Frontmatter
- [ ] YAML frontmatter has `---` delimiters
- [ ] `name` field: kebab-case, no spaces, no capitals
- [ ] `description` includes WHAT and WHEN
- [ ] No XML tags (< >) anywhere

### Instructions
- [ ] Instructions are clear and actionable
- [ ] Error handling included
- [ ] Examples provided
- [ ] References clearly linked

---

## Before Upload

### Triggering Tests
- [ ] Tested triggering on obvious tasks
- [ ] Tested triggering on paraphrased requests
- [ ] Verified doesn't trigger on unrelated topics

### Functional Tests
- [ ] Functional tests pass
- [ ] Tool integration works (if applicable)

### Packaging
- [ ] Compressed as .zip file (if uploading to Claude.ai)

---

## After Upload

- [ ] Test in real conversations
- [ ] Monitor for under/over-triggering
- [ ] Collect user feedback
- [ ] Iterate on description and instructions
- [ ] Update version in metadata

---

## Scoring Guide

Use this to calculate quality score:

| Category | Max Points | Criteria |
|----------|------------|----------|
| Frontmatter | 20 | YAML syntax valid, name/description present, no forbidden content |
| Description | 25 | WHAT + WHEN included, trigger phrases, under 1024 chars, specific |
| Structure | 20 | Under 5000 words, progressive disclosure used, critical info at top |
| Content | 20 | Error handling, examples, actionable instructions, references linked |
| Additional | 15 | references/ used appropriately, MCP integration correct |

### Score Interpretation

| Score | Grade | Meaning |
|-------|-------|---------|
| 90-100 | A | Almost no issues - production ready |
| 75-89 | B | Minor improvements available |
| 60-74 | C | Improvement recommended |
| 40-59 | D | Needs improvement |
| 0-39 | F | Fundamental issues - major rework needed |

---

## Common Deductions

| Issue | Points Lost |
|-------|-------------|
| Missing `---` delimiters | -10 |
| Name not kebab-case | -5 |
| Description missing WHAT | -10 |
| Description missing WHEN | -10 |
| Description too vague | -5 |
| No trigger phrases | -5 |
| XML tags in frontmatter | -10 |
| SKILL.md over 5000 words | -5 |
| No error handling | -5 |
| No examples | -5 |
| Ambiguous instructions | -5 |
| References not linked | -3 |
