# Resumability

## What gets lost
Switching accounts or hitting a quota ends or pauses sessions. Reboots end background processes and scheduled tasks. Context compaction loses detail. Plan for all three.

## Durable by default
- Scripts, state files, resume notes and results live in the repository or a durable folder.
- Scheduled work is described in a file (what, when, how to re-create) so it can be re-armed.
- Job ids and process ids are written down when started.

## Resume note
Keep it at the top of the status page or beside it:
- Where things stand in two lines.
- Running jobs with ids and expected end.
- The first thing to do on return.
- Things not to repeat (a step that is not idempotent, a decision already made).

## After an interruption
1. Check what survived: processes, jobs, schedules, branches, free disk. This is the first step of every round (`loop.md`), not a special procedure.
2. Read the resume note and status page.
3. Re-arm schedules and watchers; verify with a list, not by assumption.
4. Compare the repository state with the note; fix the note.
5. Continue the loop.

## Half-done work
Commit it to a branch with a message that says what is incomplete. Never leave important work only as uncommitted edits in the main checkout.

## Restartable jobs
Write progress to disk at intervals, make each step safe to run again, keep inputs immutable once a job starts.

## Concrete traps seen
- A recurring schedule has a maximum life of days; list its expiry and renew before it.
- A reboot cleared the system temp area and deleted whole worktrees: keep work in a durable folder, not temp or scratch.
- After an account switch, background jobs, scratch files and queue lists were gone and slots stayed held by dead agents: re-create, release by name.
- A pause order must be reversible by one script, with the order of switching on written down: workers, schedules, the plan, then release holds. Experiments extend by the length of the stop.
- Keep a "gate window" or similar hold flag in a file with the reason written inside it, so a returning lead knows why it is set.
- An agent that stops while holding a resource leaves the resource waiting: check for frozen work with no live agent every round.
- Long remote jobs: start detached, write a pid file, log to a file, check by script.
- Counts that feed a verdict are keyed by the experiment, not by the machine or the lane that produced them. Otherwise a change of machine, or a move of the gate, restarts every count.
