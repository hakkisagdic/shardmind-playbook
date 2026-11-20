---
description: Run one round of the judgement-only loop
argument-hint: "[focus, optional]"
---
Run one round of the judgement loop from the autonomous-project-lead skill. Focus: $ARGUMENTS (if empty, everything).

Look: first the scripted "what survived" check and a cheap smoke check; then the status page, resume note, script digests (job verdicts, machine room, merge queue, failed checks), the questions queue for new answers.
Decide: what to start, stop, merge, delegate, ask, report, or record as a lesson. Classify each action (reversible internal, user-visible, or needs the owner's word).
Never: sleep or poll by hand, retype fixed commands, count by eye, read long logs. If you are about to, write or call a script, or hand it to a watcher.
Finish: update the state files so they are true, queue any questions with a default, and report in a few lines with numbers.
