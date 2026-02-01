# MCP Integration Guidance

Based on "The Complete Guide to Building Skills for Claude" (Chapter 1, p.5-6; Chapter 2, p.8-9)

## The Kitchen Analogy

**MCP provides the professional kitchen:** access to tools, ingredients, and equipment.

**Skills provide the recipes:** step-by-step instructions on how to create something valuable.

Together, they enable users to accomplish complex tasks without needing to figure out every step themselves.

## How They Work Together

| MCP (Connectivity) | Skills (Knowledge) |
|--------------------|-------------------|
| Connects Claude to your service (Notion, Asana, Linear, etc.) | Teaches Claude how to use your service effectively |
| Provides real-time data access and tool invocation | Captures workflows and best practices |
| What Claude can do | How Claude should do it |

## Why This Matters for Your MCP Users

### Without skills:

- Users connect your MCP but don't know what to do next
- Support tickets asking "how do I do X with your integration"
- Each conversation starts from scratch
- Inconsistent results because users prompt differently each time
- Users blame your connector when the real issue is workflow guidance

### With skills:

- Pre-built workflows activate automatically when needed
- Consistent, reliable tool usage
- Best practices embedded in every interaction
- Lower learning curve for your integration

## MCP Tool Call Best Practices

### 1. Verify Tool Names

```markdown
# Good - exact tool name
Call MCP tool: `create_customer`

# Bad - assumed name
Call MCP tool: `createCustomer`
```

Tool names are case-sensitive. Check MCP server documentation.

### 2. Include Required Parameters

```markdown
### Step 1: Create Project
Call MCP tool: `create_project`
Parameters:
  - name: (required) Project name
  - team_id: (required) Team identifier
  - description: (optional) Project description
```

### 3. Handle Dependencies

```markdown
### Step 2: Create Tasks
Call MCP tool: `create_task`
Parameters:
  - project_id: Use ID from Step 1
  - title: Task title
```

### 4. Add Validation Steps

```markdown
### Step 1: Verify Connection
Before proceeding, verify MCP connection:
1. Check Settings > Extensions > [Your Service]
2. Should show "Connected" status

If not connected, guide user to reconnect.
```

## Error Handling Patterns

### Connection Errors

```markdown
## MCP Connection Failed

If you see "Connection refused":
1. Verify MCP server is running: Check Settings > Extensions
2. Confirm API key is valid
3. Try reconnecting: Settings > Extensions > [Your Service] > Reconnect
```

### Authentication Errors

```markdown
## Authentication Failed

If you see "401 Unauthorized":
1. API key may be expired - regenerate in service dashboard
2. Permissions may be insufficient - check required scopes
3. OAuth token may need refresh - reconnect the integration
```

### Tool Not Found

```markdown
## Tool Not Found

If MCP tool call fails with "unknown tool":
1. Verify tool name spelling (case-sensitive)
2. Check MCP server version supports this tool
3. Ensure required MCP server is connected
```

## Testing MCP Integration

### Test MCP Independently

Before debugging your skill, test MCP directly:

```
Ask Claude: "Use [Service] MCP to fetch my projects"
```

If this fails, the issue is MCP configuration, not your skill.

### Checklist

- [ ] MCP server is connected (Settings > Extensions)
- [ ] API keys are valid and not expired
- [ ] Proper permissions/scopes granted
- [ ] OAuth tokens refreshed (if applicable)
- [ ] Tool names match MCP documentation exactly
- [ ] Required parameters included in skill instructions
- [ ] Error handling for common MCP issues

## Skill Category for MCP Enhancement

**Category 3: MCP Enhancement**

Used for: Workflow guidance to enhance the tool access an MCP server provides.

Real example: sentry-code-review skill (from Sentry)

"Automatically analyzes and fixes detected bugs in GitHub Pull Requests using Sentry's error monitoring data via their MCP server."

### Key Techniques

- Coordinates multiple MCP calls in sequence
- Embeds domain expertise
- Provides context users would otherwise need to specify
- Error handling for common MCP issues
