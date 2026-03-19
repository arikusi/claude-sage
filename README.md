# claude-sage

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/arikusi/claude-sage/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-v2.0%2B-purple.svg)](https://code.anthropic.com)
[![Marketplace](https://img.shields.io/badge/marketplace-arikusi-orange.svg)](https://github.com/arikusi/arikusi-marketplace)

> **"Analyze First, Fix Later"** — Workflow system for Claude Code CLI

An agent-based workflow system for Claude Code that emphasizes careful analysis, strategic planning, and controlled implementation. No more quick fixes that become technical debt.

---

## A quick note before you start

**MCP Servers - Seriously, install these:** I *highly* recommend setting up the MCP servers (especially **Context7** and **Exa**). They're what make the `horizon-explorer` agent so powerful. Without them, you're missing out on:
- [Exa](https://github.com/exa-labs/exa-mcp-server): Real-world code examples, API documentation, web search
- [Context7](https://github.com/upstash/context7): Up-to-date library documentation

Trust me, these aren't optional nice-to-haves—they're game-changers.

> **Important:** Install MCPs **locally** (not as remote/cloud-hosted servers). Subagents cannot authenticate with remote MCPs and will silently fall back to no-op. Local installation is required for agents to use them.

---

## Core Philosophy

**Stop rushing into fixes. Start building right.**

This system replaces the "quick fix" mentality with a structured, multi-agent approach:
1. **Analyze** the problem deeply (no code changes)
2. **Architect** the solution strategically (ultrathink mode)
3. **Explore** alternative perspectives (when stuck)
4. **Fix** carefully with explicit approval (test-driven)

---

## Quick Start

**Option 1 — Plugin install:**

```
/plugin marketplace add arikusi/arikusi-marketplace
/plugin install claude-sage@arikusi-marketplace
```

**Option 2 — Tell Claude to install it for you:**

```bash
# Open Claude Code CLI
claude

# Then say:
"Add the marketplace and install claude-sage:
/plugin marketplace add arikusi/arikusi-marketplace
/plugin install claude-sage@arikusi-marketplace"
```

That's it! Claude will set everything up for you.

---

## The Agent Team

### 1. **problem-analyst**
- **Role**: Forensic investigator
- **Tools**: Read, Grep, Glob, Bash (read-only)
- **Powers**: Cannot write/edit code
- **Purpose**: Deep root cause analysis, impact assessment, reporting

**When to use:** `/analyze [problem description]`

### 2. **solution-architect**
- **Role**: Strategic architect
- **Model**: opus + extended thinking
- **Tools**: Read, Grep, Glob, Bash, TaskCreate/Update (read-only for code)
- **Powers**: Cannot write/edit code — produces detailed plans with step-by-step tasks
- **Purpose**: Architecture decisions, trade-off analysis, 3+ alternative approaches

**When to use:** `/architect [architectural decision]`

### 3. **horizon-explorer**
- **Role**: Lateral thinker
- **Tools**: Read, Grep, Glob, Bash, Exa MCP, Context7, WebSearch
- **Powers**: Cannot write/edit code, Research superpowers
- **Purpose**: Alternative perspectives, unconventional solutions, "are we looking in the right place?"

**When to use:** `/explore [stuck problem]`

### 4. **controlled-fixer**
- **Role**: Careful surgeon
- **Tools**: Read, Grep, Glob, Edit, Write, Bash, TaskCreate/Update
- **Powers**: Can modify code (ONLY with explicit approval, in isolated worktree)
- **Purpose**: Test-driven, reversible, minimal-impact fixes in a separate git branch

**When to use:** `/fix` (after analysis and approval)

### 5. **code-reviewer**
- **Role**: Independent quality gate
- **Model**: opus + extended thinking
- **Tools**: Read, Grep, Glob, Bash (read-only)
- **Powers**: Cannot modify code — produces APPROVED / NEEDS CHANGES / BLOCKED
- **Purpose**: Security (OWASP), logic correctness, edge cases, test coverage review

**When to use:** `/review` (after `/fix`, before merging)

---

## Quick Commands

| Command | Calls | What It Does | Modifies Code? |
|---------|-------|--------------|----------------|
| `/analyze` | problem-analyst | Deep problem analysis + report | No |
| `/architect` | solution-architect | Strategic planning (extended thinking, opus) | No |
| `/explore` | horizon-explorer | Alternative approaches | No |
| `/fix` | controlled-fixer | Careful implementation in worktree | Yes (if approved) |
| `/review` | code-reviewer | Independent review before merge | No |
| `/setup-project` | — | Initialize Claude for new project | Yes |

---

## Standard Workflows

### Workflow 1: Simple Bug Fix

```
1. /analyze "npm test failing on auth module"
   → problem-analyst: detailed analysis + root cause

2. You: "Okay, proceed with the fix"
   → /fix
   → controlled-fixer: test-driven fix in isolated worktree

3. /review
   → code-reviewer: APPROVED

4. git merge [branch]
```

### Workflow 2: Critical Architectural Decision

```
1. /analyze "should we migrate to microservices?"
   → problem-analyst: current state analysis

2. /architect "microservices migration strategy"
   → solution-architect: 3+ approaches, trade-offs, risks

3. You: decide which approach

4. /fix [chosen approach]
   → controlled-fixer: careful implementation in worktree

5. /review
   → code-reviewer: review + merge decision
```

### Workflow 3: Stuck for Hours

```
1. /explore "we can't solve the race condition in our queue"
   → horizon-explorer: alternative patterns, different tech stacks
   → "Are we looking in the right place?"

2. /analyze [new perspective]
   → problem-analyst: re-analyze with new insights

3. /architect [solution]
   → solution-architect: strategic plan

4. /fix → /review → merge
```

---

## Best Practices

### DO

1. **Always start with /analyze** - Never skip understanding
2. **Use /architect for critical changes** - Get strategic guidance
3. **Call /explore when stuck** - Fresh perspective
4. **Approve before /fix** - Controlled changes only
5. **Request "ultrathink"** - For complex decisions

### DON'T

1. **Skip analysis** - Understanding > Speed
2. **Quick fixes** - Today's shortcut = Tomorrow's debt
3. **Single perspective** - Use /explore when uncertain
4. **Untested production changes** - controlled-fixer enforces tests
5. **Breaking changes without warning** - Agents will flag

---

## Features

### Intelligent Permissions
```json
{
  "allow": ["Bash(git:*)", "Bash(npm:*)"],
  "deny": ["Bash(rm:-rf:*)"]
}
```

### Post-Tool Hooks
```json
{
  "PostToolUse": [{
    "matcher": "Edit|Write",
    "hooks": [{"command": "prettier --write $FILE"}]
  }]
}
```

### Extended Thinking
```json
{
  "env": {
    "MAX_THINKING_TOKENS": "31999"
  }
}
```

### MCP Integration

The workflow system is *significantly* more powerful with MCP servers:

- [Exa](https://github.com/exa-labs/exa-mcp-server): Real-world code examples, API documentation, web search
- [Context7](https://github.com/upstash/context7): Up-to-date library documentation

**Without MCPs:** Basic workflow works, but `horizon-explorer` loses 80% of its research capabilities.
**With MCPs:** Full-powered alternative research, fresh code examples, and comprehensive documentation.

> **Install MCPs locally.** Subagents (spawned via the Agent tool) cannot authenticate with remote/cloud-hosted MCP servers — they will silently fail to use them. All MCP servers must be installed and running locally on your machine for the agents to access them reliably.

---

## Documentation

- **WORKFLOW-GUIDE.md**: Comprehensive workflow documentation
- **Agent files**: Each agent has detailed instructions
- **Command files**: Slash command definitions

---

## Agent Decision Matrix

| Scenario | Agent | Why |
|----------|-------|-----|
| "Why is this failing?" | problem-analyst | Analysis without changes |
| "Should we use GraphQL or REST?" | solution-architect | Strategic trade-offs |
| "We've been stuck for 3 hours" | horizon-explorer | New perspective |
| "Implement the approved fix" | controlled-fixer | Careful execution |
| "Cron job failing intermittently" | problem-analyst → architect | Race conditions need deep analysis |
| "Database migration strategy" | architect | Critical architectural decision |

---

## Advanced Usage

### Custom Agents

Add your own agents to `~/.claude/agents/`:

```markdown
---
name: your-agent
description: What it does
tools: Read, Grep
disallowedTools: Write, Edit
model: sonnet
---

# Your Agent Instructions

...
```

---



---

*"Quick fix today, big problem tomorrow."*

Understand the problem first. Fix it once, fix it right.

---

## 📜 License

MIT
