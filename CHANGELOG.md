# Changelog

## [1.0.0] - 2026-03-19

### Agents
- **problem-analyst**: Deep analysis, read-only, never touches code
- **solution-architect**: Strategic planning, opus + extended thinking, produces step-by-step task plans
- **horizon-explorer**: Alternative perspectives and research via Exa and Context7
- **controlled-fixer**: Careful implementation in isolated worktree, requires explicit approval
- **code-reviewer**: Independent post-implementation review (opus, extended thinking), returns APPROVED / NEEDS CHANGES / BLOCKED

### Commands
- `/claude-sage:analyze` — calls problem-analyst
- `/claude-sage:architect` — calls solution-architect
- `/claude-sage:explore` — calls horizon-explorer
- `/claude-sage:fix` — calls controlled-fixer
- `/claude-sage:review` — calls code-reviewer
- `/claude-sage:setup-project` — initializes Claude config for new projects

### Plugin
- Distributed via [arikusi-marketplace](https://github.com/arikusi/arikusi-marketplace)

[1.0.0]: https://github.com/arikusi/claude-sage/releases/tag/v1.0.0
