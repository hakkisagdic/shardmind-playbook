# Shared-machine etiquette

## Before heavy work
Run a room check (memory, CPU load, free disk, GPU use). Treat a "no" as final for now. For a full test suite or other long heavy job, take a slot from whatever slot mechanism the machine has, and release it afterwards even on failure.

## Announcing
A short note in a shared place: who, what, expected length, what it touches. Remove or update it when done.

## Processes
- Record the exact id of every process you start.
- Stop only those, by id.
- Never kill by name pattern and never pipe a listing into a kill: it hits other people's work.
- If something you did not start is in the way, ask its owner or the coordinator.

## Cleanup
Remove temp files, scratch worktrees, containers, caches you created, and finished background jobs. Use the safe-delete helper if one exists.

## Disk
- Know the budget; check free space before large writes.
- Checkpoints, rollouts and logs are the usual growth. Keep the last few, compress or move the rest, record where they went.
- Delete only what can be regenerated or has a verified copy elsewhere.

## Coordinator
One party decides priorities and GPU or CPU time among projects. You request, you state expected benefit, you accept the answer. If your work has no current priority, do light, reversible, useful work.

## Idle resources
Granted capacity left idle is waste. Keep a ready list of useful reversible jobs for free or already-paid capacity, within the owner's budget, and start them when the check allows.

## Measuring pressure
Judge memory by the operating system's own pressure report and by the paging rate (pages swapped in per second against that machine's quiet baseline), not by swap size or by free memory derived by hand: swap files stay large after heavy jobs, so a gate on size is always red, and a gate that is always red is ignored. Check every few steps during a long job, because pressure builds slowly.

## Claims
A claim is a note that tells others whom to ask when the machine is tight. Claim before long heavy work, release after. A full test suite also takes a numbered slot from the machine's slot mechanism if it has one; release by name if an agent died holding one.

## Sharing a GPU or CPU with someone else's work
- Take a baseline of their workload alone.
- Measure each share before cutting: switch your consumers off one at a time for a short fixed window and record their rate. The obvious suspect may be innocent.
- Agree budgets in writing: core limits by time of day, lower scheduling weight, workers no more than cores, a one-line notice to the machine's owner before starting.
- Cut long jobs into chunks, each producing its own output, so a pause costs one piece.
- Wait for a service's ready message after any restart.
- In real-time comparisons give each arm its own worker.

## Restarts
Restarting the machine is the machine owner's call, not an agent's. Stop dependent services in order, expect schedules and background jobs to vanish, rebuild from durable folders, and check what survived.
