---
description: Refresh the project status page with its six fixed headings
argument-hint: "[path to the status page, optional]"
---
Refresh the status page using the autonomous-project-lead skill. Target: $ARGUMENTS (if empty, use the project's state folder, or create it from `templates/status.md` in the autonomous-project-lead skill folder).

1. Read the current page, open items, questions queue and the latest summaries from scripts. Do not read raw logs.
2. Rewrite the page, keeping these headings in this order: Goal and date; Running jobs; Next items; What the owner owes; Unpushed work; What I need from you.
3. Use today's date, measured numbers and ids. Write "none" for empty sections.
4. Check unpushed work against the repository (branches and commits not on the remote).
5. Reply with at most five lines: what changed and what you need.
