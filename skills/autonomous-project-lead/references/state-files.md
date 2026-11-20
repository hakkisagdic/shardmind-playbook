# State files in detail

## Status page
One page, overwritten, never appended. Someone who has not seen the project for a week must understand it in one minute.

- **Goal and date**: the one-sentence goal, today's date, the phase you are in.
- **Running jobs**: each with what it is, where it runs, when it started, when you expect it to end, and where its output lands. Empty means say "none".
- **Next items**: the next three to five things, in order, each one line.
- **What the owner owes**: reviews, approvals, decisions already queued and overdue. Be specific about what is blocked by it.
- **Unpushed work**: branches and commits that exist only locally, so a disk loss or a switch cannot hide them.
- **What I need from you**: the shortest list of asks. If empty, say so; that is good news.

Add an "Expected rate" to each running job (units per second or CPU-hours), so a stalled job shows against it. Fresh numbers beat prose. Link to the experiment record rather than retelling it.

## Open items
One line per item: id, what, who or which agent, next step, blocked-by. Close items by moving them to a done section with the date, not by deleting.

Keep acceptance in a machine-readable list beside the page (`templates/acceptance.json`): one record per deliverable (`id`, `check`, `status`, `evidence`). Agents may change only the status and the evidence, and only with the check's output attached. The merge helper refuses a change to this file that touches anything else; adding and closing records is the owner's or the lead's job, in a separate commit, and the open-items page is rendered from it. Generate what a script can know (running jobs, unpushed work, schedules, slots held); hand-written status goes stale. Review the whole list at least weekly and drop what no longer matters, saying why.

## Operating lessons
Append-only, newest on top. Each entry:
- Date and a short title.
- What happened (facts, numbers, timeline).
- How it was noticed, and how late (a check, the scan, or a person).
- Contributing factors. An incident needs several things to line up; naming one real cause hides the others, and the next incident uses them. Include what made it hard to see.
- What went well, and where we were lucky.
- Actions, each typed (detect, mitigate, prevent) and with an open-item id.
- Rule (a sentence that would have prevented it) and the mechanism that carries it (the script, check or hook).
Group by theme occasionally so repeated lessons become visible. A lesson that repeats means its enforcement was too weak.

## Daily diary
One file per day, dated name. Contents: what ran, what finished, numbers, decisions and their reasons, and exactly one lesson, the thing you would tell tomorrow's self. Pick one even on a quiet day. The one-lesson limit forces you to decide what mattered.

## Resume note
See `references/resumability.md`; the template is in this skill's `templates/` folder.

## Questions queue
One entry per question: id, date asked, the question in plain words, the options, your recommendation, the default you will take and when, and the status (open, answered, defaulted). Answers are recorded with their date. When the owner answers in chat, copy the answer into the queue so it survives.
