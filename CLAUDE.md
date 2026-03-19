# claude-sage — Project Context

This repo is a Claude Code plugin. It provides a multi-agent workflow system distributed via the arikusi-marketplace.

## What this project is

Five agents with specific roles, loaded as a Claude Code plugin:

- `problem-analyst` — analysis only, never touches code
- `solution-architect` — strategic planning, opus + extended thinking
- `horizon-explorer` — alternative research, has Exa + Context7 access
- `controlled-fixer` — implements approved fixes in isolated worktree
- `code-reviewer` — independent review before merge, opus + extended thinking

Commands are prefixed with `claude-sage:` when installed as a plugin (e.g. `/claude-sage:analyze`).

## Distribution

Installed via:
```
/plugin marketplace add arikusi/arikusi-marketplace
/plugin install claude-sage@arikusi-marketplace
```

Marketplace repo: https://github.com/arikusi/arikusi-marketplace

## File structure

```
agents/          # Agent definitions (.md with frontmatter)
commands/        # Slash command definitions (.md)
.claude-plugin/
  plugin.json    # Plugin manifest (name, version, description)
settings.template.json  # Suggested settings for users
WORKFLOW-GUIDE.md       # Detailed workflow documentation
CHANGELOG.md            # Version history
```

## MCP dependencies

`horizon-explorer` uses Exa and Context7. These must be installed locally — agents can't authenticate with remote MCP servers.
