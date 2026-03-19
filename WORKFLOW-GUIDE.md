# claude-sage — Workflow Guide

## Core Principle

**"Analyze First, Fix Later"**

Understand deeply before changing anything. A correct fix is better than a quick fix.

---

## Standard Problem-Solving Workflow

### 1. Analysis Phase (never skip)

```
/claude-sage:analyze
```

**What happens:**
- problem-analyst is invoked
- Read-only access (no Write/Edit)
- Deep root cause analysis
- Impact assessment
- List of recommended approaches

**Output:**
- Detailed analysis report
- Classification: Simple / Critical / Uncertain
- Suggested next step

### 2. Decision Phase

**If it's a simple fix:**
```
You: "OK, apply the fix"
→ controlled-fixer is invoked
```

**If it's critical / architectural:**
```
/claude-sage:architect
```
- solution-architect is invoked (opus model, extended thinking)
- 3+ alternative approaches
- Trade-off analysis
- You decide

**If uncertain / stuck:**
```
/claude-sage:explore
```
- horizon-explorer is invoked
- Alternative perspectives
- Different technologies researched
- "Are we looking in the right place?" question

### 3. Implementation Phase (requires your approval)

```
/claude-sage:fix
```

**Pre-conditions:**
- [ ] Analysis completed
- [ ] Plan made (if critical)
- [ ] You explicitly approved

**What happens:**
- controlled-fixer is invoked in an **isolated worktree** (separate git branch)
- Test-driven changes
- Small, atomic commits
- Rollback plan ready
- Reports the branch name when done

### 4. Review Phase (after fix, before merge)

```
/claude-sage:review
```

**What happens:**
- code-reviewer is invoked (opus model, extended thinking)
- Inspects git diff: security, logic, edge cases, tests
- Returns APPROVED, NEEDS CHANGES, or BLOCKED

**Merge decision:**
- APPROVED → `git merge [branch]`
- NEEDS CHANGES → your call whether to merge
- BLOCKED → send back to controlled-fixer

---

## Critical Scenarios

### Cron Jobs / Pipeline Jobs

1. `/claude-sage:analyze` → problem-analyst analyzes in detail
2. Examine job logs, schedule, dependencies
3. If timing/concurrency issue: `/claude-sage:architect`
4. With extended thinking:
   - Race condition analysis
   - Retry logic
   - Idempotency check
   - Monitoring strategy

### Architectural Changes

1. `/claude-sage:analyze` → identify the problem
2. `/claude-sage:architect` → 3 alternative plans
3. `/claude-sage:explore` → how do other systems do it? (optional)
4. You decide
5. `/claude-sage:fix` → careful implementation
6. `/claude-sage:review` → verify before merge

### Stuck for Hours

1. `/claude-sage:explore` → call horizon-explorer
   - "Are we looking in the right place?"
   - Alternative approaches
   - Cross-domain solutions
2. `/claude-sage:analyze` again with fresh perspective
3. `/claude-sage:architect` with new plan

---

## Agent Collaboration Patterns

### Standard debug

```
problem-analyst
    ↓ (simple fix)
controlled-fixer → worktree
    ↓
code-reviewer
    ↓ (approved)
merge
```

### Critical change

```
problem-analyst
    ↓
solution-architect (extended thinking)
    ↓
controlled-fixer → worktree
    ↓
code-reviewer
    ↓ (approved)
merge
```

### Uncertain problem

```
problem-analyst
    ↓ (uncertain)
horizon-explorer
    ↓ (new perspective)
problem-analyst (again)
    ↓
solution-architect
    ↓
controlled-fixer → worktree
    ↓
code-reviewer → merge
```

---

## Quick Command Reference

| Command | Agent | Purpose | Modifies Code? |
|---------|-------|---------|----------------|
| `/claude-sage:analyze` | problem-analyst | Deep analysis & report | No |
| `/claude-sage:architect` | solution-architect | Strategic planning (extended thinking) | No |
| `/claude-sage:explore` | horizon-explorer | Alternative approaches & research | No |
| `/claude-sage:fix` | controlled-fixer | Careful implementation (worktree) | Yes (with approval) |
| `/claude-sage:review` | code-reviewer | Post-implementation review | No |

---

## Settings Overview

**Configuration:**
```json
{
  "model": "sonnet",
  "alwaysThinkingEnabled": true,
  "permissions": {
    "defaultMode": "default",
    "disableBypassPermissionsMode": "disable",
    "allow": ["Bash(git:*)", "Bash(npm:*)", "..."],
    "deny": ["Bash(rm:-rf:*)", "Bash(git:push:--force:*)", "..."]
  },
  "env": {
    "MAX_THINKING_TOKENS": "31999"
  }
}
```

**MCP Servers:**
- [Context7](https://github.com/upstash/context7) — library documentation
- [Exa](https://github.com/exa-labs/exa-mcp-server) — web search and code examples
- [deepseek-mcp-server](https://github.com/arikusi/deepseek-mcp-server) — optional

---

## Best Practices

### DO

1. **Always start with /analyze**
2. **Use /architect for critical changes**
3. **Use /explore when stuck**
4. **Never run /fix without your approval**
5. **Always run /review before merging**

### DON'T

1. **Don't ask for quick fixes** — quick fix = technical debt
2. **Don't skip analysis** — don't change what you don't understand
3. **Don't stay stuck on one perspective** — use /explore
4. **Don't push to production without tests**
5. **Don't merge without /review**

---

## Tips

### Proactive architecture review
Before starting a large feature:
```
/claude-sage:architect
# "What's the best approach for adding feature X?"
```

### Alternative research
When evaluating new technologies or patterns:
```
/claude-sage:explore
# "What are the modern best practices for problem Y?"
```

### Postmortem analysis
After fixing a bug:
```
/claude-sage:analyze
# "How did this bug occur and how do we prevent recurrence?"
```

### Pre-PR review
Before opening a pull request:
```
/claude-sage:review
```

---

## Troubleshooting

```bash
# List installed plugins
claude plugin list

# Update marketplace
/plugin marketplace update arikusi-marketplace

# Reinstall
/plugin uninstall claude-sage@arikusi-marketplace
/plugin install claude-sage@arikusi-marketplace
```
