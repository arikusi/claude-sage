---
description: Carefully implement an approved change (in isolated worktree)
---

Before using this command, verify:
- [ ] Has problem-analyst reported?
- [ ] Has solution-architect planned (if critical)?
- [ ] Has the user explicitly approved?

If all the above are confirmed, call the controlled-fixer agent.

Make careful, test-driven, reversible changes in an isolated worktree.

If you see a red flag (10+ files, core module, etc.) ask the user first!

After completing, remind the user to run /review before merging.
