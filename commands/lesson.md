---
description: Write an operating lesson after an incident
argument-hint: "<what happened>"
---
Write an operating lesson for: $ARGUMENTS, using `templates/lesson.md` from the autonomous-project-lead skill folder.

Gather facts first (what happened, numbers, timeline) from files. Find the contributing factors (several, not one "real cause"), how the incident was noticed and how late, and where luck helped. Write the rule that would have prevented it, and then the enforcement: the script, check or setting that now carries it (create or change it only inside the project; permission settings, hooks and shared-machine settings are proposed to the owner first, never changed on your own). Add the entry on top of the lessons file. If it repeats an earlier lesson, say so and strengthen the enforcement. If you were wrong about something, say what was wrong, how big, and what changed.
