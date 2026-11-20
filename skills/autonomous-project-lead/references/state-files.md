# State files in detail

## Status page
One page, overwritten, never appended. Someone who has not seen the project for a week must understand it in one minute.

- **Goal and date**: the one-sentence goal, today's date, the phase you are in.
- **Running jobs**: each with what it is, where it runs, when it started, when you expect it to end, and where its output lands. Empty means say "none".
- **Next items**: the next three to five things, in order, each one line.
- **What the owner owes**: reviews, approvals, decisions already queued and overdue. Be specific about what is blocked by it.
- **Unpushed work**: branches and commits that exist only locally, so a disk loss or a switch cannot hide them.
- **What I need from you**: the shortest list of asks. If empty, say so; that is good news.

Fresh numbers beat prose. Link to the experiment record rather than retelling it.

## Open items
One line per item: id, what, who or which agent, next step, blocked-by. Close items by moving them to a done section with the date, not by deleting. Review the whole list at least weekly and drop what no longer matters, saying why.

## Operating lessons
Append-only, newest on top. Each entry:
- Date and a short title.
- What happened (facts, numbers).
- Cause (the real one, not the first guess).
- Rule (a sentence that would have prevented it).
- Enforcement (the script, check or hook that now carries the rule).
Group by theme occasionally so repeated lessons become visible. A lesson that repeats means its enforcement was too weak.

## Daily diary
One file per day, dated name. Contents: what ran, what finished, numbers, decisions and their reasons, and exactly one lesson, the thing you would tell tomorrow's self. Pick one even on a quiet day. The one-lesson limit forces you to decide what mattered.

## Resume note
See `references/resumability.md`; the template is in this skill's `templates/` folder.

## Questions queue
One entry per question: id, date asked, the question in plain words, the options, your recommendation, the default you will take and when, and the status (open, answered, defaulted). Answers are recorded with their date. When the owner answers in chat, copy the answer into the queue so it survives.
