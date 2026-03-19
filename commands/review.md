---
description: Independently review the last set of changes (security, logic, quality)
---

Call the code-reviewer agent.

Inspect controlled-fixer's recent changes (git diff):
1. Security analysis (OWASP checklist)
2. Logic correctness and edge cases
3. Code quality
4. Test sufficiency
5. Performance implications

Return APPROVED, NEEDS CHANGES, or BLOCKED.
The merge decision belongs to the user.
