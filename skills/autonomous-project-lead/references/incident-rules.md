# Incident rules: what went wrong, the rule that prevents it

Each entry is a kind of incident met in one long autonomous project and on the shared machine it ran on, generalised. Format: **what went wrong**, then **rule**, then the **reason** it holds. Some incidents were the lead's own, some other agents', some the machine's. Treat the list as a checklist when you design scripts, schedules and briefs, not as a universal law. Any number in it is a round illustration.

## A. Schedules, watchers and things that silently stop

**1. The hourly check vanished and nobody noticed for hours.**
A recurring schedule has a maximum life (days). It expired quietly, the wake-up watcher was never re-created, and a resource sat idle for hours until the owner noticed.
Rule: every recurring schedule is listed in a file with its expiry and its re-creation command; the loop checks "do my schedules still exist" each round; a watcher is re-armed at the end of every event it handled.
Reason: absence of an alarm looks identical to a quiet system.

**2. Alarms alone are not monitoring.**
All checks relied on events firing. When the event source died, nothing fired.
Rule: run two independent layers: alarms (fast, event-driven) and a periodic scan that looks for expected-but-missing evidence (a lane with no heartbeat, a job with no output for too long). The owner's standard: the lead catches problems, the owner does not.
Reason: the scan finds what the alarm cannot know it missed.

**3. A watcher or helper that holds "paused" state in memory undid a manual pause.**
A periodic guard held its settings only in its own memory and periodically wrote them back, erasing a manual pause.
Rule: shared control state lives in a file, read before every write, written under a lock, by loop and humans alike.
Reason: two writers with private copies overwrite each other.

**4. A false alarm after a restart or sleep taught everyone to ignore alarms.**
Silence timers counted from the wrong origin (before the process restarted, before the machine woke).
Rule: measure staleness from the later of "process start" and "machine's last wake". Test each alarm with a restart and a sleep.
Reason: a noisy alarm is worse than none; it trains the reader to skip the real one.

**5. Pausing in the wrong order left a worker without its place.**
Emptying a shared plan while a consumer was mid-round made that consumer reassign a seat wrongly, and the damage took hours to show.
Rule: write the stop and resume order down (stop consumers first, then the plan, then workers; resume in reverse). Make the resume order a script.
Reason: the order of switching things off is part of the design.

**6. A paused resource stayed paused because the agent who paused it stopped.**
An agent paused a shared job, then hit its quota and died. The job was frozen for hours with no one running.
Rule: the loop looks for "frozen process and no live owner" every round. Agents that pause something write who, why and when to the status page.
Reason: a held resource needs a living holder.

**7. A precautionary pause caused the harm it feared.**
A lead paused jobs because a laptop battery was low; the owner plugged in soon after; the pause had already discarded work.
Rule: do not pre-empt a failure that the system's own guard will handle. Shut down on the real event, by the component that sees it.
Reason: preventive interruptions have a cost; check whether the risk is real first.

**8. Reminders repeated because a watcher was forgotten.**
Items awaiting approval were re-announced to the owner because the watcher that notices new items was not re-created after an event.
Rule: notify only on new items; the watcher is a background wait that wakes on a new item and is re-created after every event it handled. Whatever needs the owner goes to whatever channel they asked for (if they set one up), short, saying plainly when it needs them at the computer. Chat text alone may not be seen.
## B. Machine, memory, disk, processes

**9. A bulk deploy restarted several memory-hungry processes at once and nearly exhausted memory.**
Rule: restart heavy members one at a time (staggered) while any large build runs; deployment tools get a staggered mode.
Reason: peak memory is the sum of simultaneous loads, not the average.

**10. Measuring memory with the wrong number.**
Figures derived by hand from raw page counts gave a wrong picture, and total swap size stayed high at rest because the system keeps its swap file.
Rule: judge memory by the operating system's own pressure report and by the paging rate (how fast pages are being swapped in), measured against a baseline for that machine. Never gate on swap size. A gate that is always red gets ignored, and an ignored gate is worse than none.

**11. Killing processes by pattern hit someone else's work.**
A pattern-based kill, and a stop of processes assumed to be one's own leftovers, ended work belonging to others.
Rule: stop only processes you started, by a process id recorded when you started them (the launcher writes a pid file). Never kill by name pattern. Before stopping anything unfamiliar, trace its parent and working directory; if it is not yours, give its owner the list and let them decide. Install a guard that rejects pattern kills, and treat any of your own scripts that still use patterns as bugs.

**12. Background jobs chained with `&&` held the remote session open.**
`a && b && nohup c &` backgrounds the whole chain and the session waits for it.
Rule: start a remote background job as its own command, detached with its own session and all streams redirected, with no input attached. Killing the local client does not kill a detached remote job.

**13. A pattern like /dev/tcp worked in one shell and "refused" in another.**
Rule: check what shell your tool runs before using shell-specific features; test connectivity with a short script in a language with a real socket API.

**14. Many parallel builds collided on a package manager's first-run step.**
Several builds started together after a restart and failed on a half-created config file.
Rule: when the symptom is a file-exists race during first-time initialisation, rerun once before calling it a test failure; do a single warm-up restore first when starting many builds.

**15. Orphaned test processes accumulated and the machine needed rebooting.**
Rule: a test run that leaves anything running counts as failed. Each agent shuts down whatever servers, watchers and containers it launched before it reports done, and someone watches for buildup and traces it to its source. Restarting the machine hides the leak instead of fixing it, and only the machine's owner decides on it.

**16. Disk filled slowly and nobody noticed for days.**
Free space shrank over a week from several small leaks: per-suite build images, downloads never removed, many temp folders, worktrees left after merges.
Rule: tests and scripts clean their temp folders on exit; remove a worktree the moment its branch lands; delete your own rejected, regenerable artefacts the same day, through a trash area if the machine has one; keep one copy of each artefact; announce jobs that will write a lot of data; report folder totals regularly; free space is part of the room check. Never run machine-wide pruning of images, caches or containers: only remove what you created.

**17. A cleanup rule deleted things other jobs still used.**
A scheduled tidy removed local copies that a long-running job was reading, and the job died; another rule removed remote items that mattered.
Rule: every cleanup rule lists what is protected and is tested against "is anything reading this now"; long jobs copy their inputs into their own folder; cleanup of anything not trivially regenerable goes through a trash area, not deletion.

**18. A storage quota filled and an upload failed.**
Rule: monitor the quota; a keep-list that never empties is a bug; archive and then delete what an evaluation already rejected.

**19. Restarting a stuck host mid-write corrupted state.**
A stuck container manager was force-restarted while a service inside it was writing; its saved files came back empty and the service looped while still showing "up".
Rule: stop services (so writes flush) before restarting their host; a health check must test that the service answers, not that it is up; take automatic backups of state and test a restore.

**20. A sleeping laptop spoiled real-time measurements.**
With the lid shut the machine slept, woke briefly, and clocks jumped; runs ended exactly at wake times and counted as results.
Rule: every timed round compares a wall clock with a monotonic clock and the last wake time before and after; a round during which the machine slept is thrown away and replayed, never counted. Experiments extend by the time lost (see the ML skill).

**21. Heavy load invalidated measured latency.**
Load from other projects' tests and a game server starved the system; most runs were flagged not comparable.
Rule: plan real-time work for a small fixed number of lanes in total (fewer lanes of clean data beat many noisy ones); an experiment takes the place of another lane group, not extra capacity; flag runs automatically and alarm when the flagged share is high.

## C. Merges, branches, worktrees

**22. A merge failed silently and the branch was then deleted.**
A one-liner searched the merge output for two words, saw neither, went on, and force-deleted the branch.
Rule: after a merge, verify that the branch tip is an ancestor of main; delete with the safe form that refuses unmerged work; never grep for success, read the exit code and the final line.

**23. Dirty tracked file in main blocked the queue.**
A helper refuses to start with uncommitted tracked changes. One stray edit made four queued merges refuse in seconds.
Rule: commit or stash everything before the queue starts; routine-generated files are committed by the helper under a message that names the routine; do not edit main while a chain runs, work in a worktree and add the work to the queue.

**24. A commit on main during a merge's tests forced reruns.**
The helper rightly declines to push when main has advanced since it began, but each rerun was expensive under load.
Rule: nobody commits to main during a chain; announce it to agents who use the main checkout.

**25. A merge conflict left in main broke the running service.**
Rule: abort a conflicting merge at once, resolve in the branch's own worktree by merging main into it, and trial-merge every branch with main before the chain begins.

**26. A merge whose tests broke stayed in main, unpushed; the next push carried it.**
Rule: before every push, list the commits that main has beyond the remote; a failing merge is undone or fixed immediately, never left.

**27. Worktree agents could not merge into main and the merge helper then deleted their worktree.**
Rule: tell agents "your last step is to leave the merge to the lead"; the lead merges with the helper; an agent that must continue after the merge needs a fresh worktree.

**28. Watchers pointing into a worktree broke when the merge deleted it.**
Rule: watchers and long scripts use main's paths, never a worktree's.

**29. Registry file merged badly across branches.**
Many branches appended to the same JSON registry; plain merges conflicted on every pair.
Rule: merge shared registries per record (see integration reference); a different change to the same record on both sides is a named conflict, not a silent pick.

**30. Merge messages with non-ASCII text were refused.**
Rule: write commit messages in the project's one language, the way the helper requires; keep that rule in the helper, not in your head.

**31. The queue's own files lived in a scratch folder that an account switch emptied.**
Rule: queue list, queue script and pid file live in a durable folder in the repository's working area, started detached with a pid file.

## D. Accounts, quotas, agents

**32. A weekly quota ended while agents were mid-work.**
Agents died; resources they held stayed held; hours were lost.
Rule: check the quota before starting a wave of agents; keep a state file of "who holds what"; resume stopped agents by message; release a dead agent's heavy slot by name. Follow the owner's stated tempo; at the first rate-limit error cut to one or two agents.

**33. An account switch dropped all schedules, background jobs and scratch files.**
Rule: after every switch run the "what survived" check, re-create schedules, and put anything long-lived in a durable folder started with a pid file.

**34. Agents asked about the same pending items again and again, or died together when the network dropped.**
Rule: agents can be resumed by message; work that is independent waits for the network and continues; chains that call external services retry with a ledger so a batch is never submitted twice, and a spending cap is checked before each submission against worst case.

**35. Idle capacity went unnoticed until the owner asked.**
A restarted worker spent hours producing data that nothing used.
Rule: whenever workers start (resume, restart, new machine), go through each running job in turn and ask what decision it feeds. Stop what feeds nothing, repurpose what could do better, say so in one line. Using idle capacity applies only to capacity that is free or already paid for, within the owner's budget; anything that spends new money is class three. Update the schedule's prompt in the same step, or it will put back what you parked.

**36. A tidy rule or scan has to know about the pause.**
Rule: when you park something, change every place that would re-create it (schedule text, queue, cron, guard).

## E. Correctness of the work itself

**37. Same filename, different content: the wrong dataset was trained.**
A training step cached downloads by filename; several teachers' datasets shared one name; the jobs trained on an old set and two wrong conclusions followed.
Rule: identify cached inputs by content hash or full path, never by name; before concluding from a result, read the model's own record of what it was trained on.

**38. A silent fallback imitated a result.**
Old opponents failed to load and a scripted bot played in their place; "exploiters" learned to beat the bot and looked successful.
Rule: a fallback must be loud (counted, logged, alarmed); when two measurements of the same thing disagree, find the cause before reporting either.

**39. A small observable pointed to a large bug.**
Training slowed steadily; profiling showed most time spent counting items in one container, which held huge numbers of duplicates that the environment kept adding.
Rule: when slowness or a strange rate appears, profile before theorising and trust a sample of real states over reasoning. Compare simulator and reality as distributions; when they differ, find which one is wrong before adapting either.

**40. The first suspect was not the cause.**
Priority changes and extra cores did nothing; the real cause was two processes waiting for each other.
Rule: measure (counters, stack sampling, a balanced experiment), then optimise. Allocation and waiting are suspects before CPU.

**41. A speed-up proposal changed the game being measured.**
A shorter round limit would have doubled throughput but measured a different game than the one that matters.
Rule: any change that speeds measurement must be proved to leave the measured thing unchanged (matched control, byte-identical outputs under a fixed seed). The owner or the real use case defines what "the game" is.

**42. Idle expensive hardware while most data was thrown away.**
Rule: look for the bottleneck, not for more machines.

**43. Offline agreement did not predict real strength.**
Models that matched a teacher closely lost in real play.
Rule: the judge is play in the real environment through the gate; agreement is a diagnostic.

**44. Wrong numbers stated confidently.**
When you find your own earlier claim wrong (a process you thought was yours, a count you misread), say so in the report, with the corrected number and the reason.

## F. Method

**45. An experiment nobody read was only a setting.**
Rule: register every experiment with its reading time and have a script read it; if a record is overdue and still unread a day later, raise an alert.

**46. Repeated looks inflated false positives.**
Fixed-size tests repeated at every look make the true error rate exceed its label.
Rule: use a sequential test with boundaries made for repeated looks, and raise the bar with the number of candidates tried against the same incumbent.

**47. The promotion gate promoted a copy of the incumbent.**
Rule: A/A probes; see the ML skill.

**48. Extra tools (scoreboards) must refuse a number without its count and interval.**

**49. State the success bar before building.** Register a value test for a new tool before its code exists: the level it must reach, in what budget, or the work stops.

**50. Tools need tests too.** Small infrastructure bugs (state in memory, a freeze on a canary, a dropped session, a default timeout under load) cost hours each and looked like training problems. Each fix: a few lines and a test. Give test timeouts headroom for load.
