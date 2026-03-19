---
description: Create Claude configuration for a new project
---

Create a Claude Code configuration for a new project in the current directory:

1. Create the `.claude/` directory
2. Write `.claude/CLAUDE.md` with project rules:
   - Technology stack
   - Code style rules
   - Important directory structure
   - Project-specific instructions

3. Create `.claude/settings.json`:
   - Model preference
   - Permissions
   - Hooks

4. Create `.mcp.json` (for project-specific MCPs if needed)

5. Add to `.gitignore`:
   ```
   .claude/settings.local.json
   .claude/*.lock
   ```

Analyze the existing project structure and make recommendations.
