# Integration and the merge queue

## Preconditions for starting a chain
- Main checkout clean: every tracked change committed. A merge helper should refuse to start on a dirty tree, and that refusal is correct.
- Machine room checked: each merge runs its suites.
- Branch list ordered: independent small ones first, risky and wide ones later, ones that touch the same files adjacent so conflicts surface early.

## During a chain
- Nobody commits to main. Tell agents who work in the main checkout. Anything they need to record goes through the shared tools that the merge step knows about, or waits.
- If main moves during a merge's checks, the final push must refuse; rerun that merge. That refusal protects you.
- One script runs the chain and stops at the first merge that does not end in a clear success line. You then decide; the script does not improvise.

## Trial merges
Compute the merge without keeping it (a tree-only merge). It reports conflicts and which files. Use it to decide the order and to brief a developer who has to rebase.

## Record-level merges for shared registries
When many branches add records to one JSON file:
- Identify records by id.
- Merge per record: if only one side changed a record, take it; if both sides made the same change, keep it; if both changed it differently, report a conflict naming the ids and keep the current side until a human or lead resolves it.
- Install the merge rule in the repository's attributes and have the merge helper register it, so every merge and every trial merge uses it.
- Add a check that the merged file still parses and ids are unique.

## Deleting branches
After the merge lands: confirm the branch tip is contained in main, confirm the remote has the merge, then delete the local branch and its worktree. If anything is uncertain, keep the branch. A branch is cheap; lost work is not.

## Generated files
Registries, cost logs and index files that scripts rewrite should be committed by the merge helper, not left as stray modifications that block the next merge.

## What a good merge helper does (semantics, no code)
In order, each step only if the one before succeeded:
1. Refuses unless main is checked out, no merge is under way, no tracked file is modified (routine-generated files are named in a list and committed first under a message naming the routine), the branch has its own commits, and main is not behind the remote (fetch first).
2. Merges with a merge commit (never fast-forward) and an English message. A conflict in a routine-generated file takes main's copy where the list says so and says so in the message; any other conflict stops with the files listed and the merge left under way for a person. It never resolves or aborts on its own.
3. Verifies the merge commit has two parents and the branch is an ancestor of main.
4. Runs the test suites the change touches, one process per module, once the machine-room check passes, with a claim announced while they run. Selection: the suites for changed code, suites that import a changed module (followed through the project), docs and CI suites always. A failing suite stops it with the merge left, unpushed.
5. Pushes (if the remote is public, pushing follows the owner's policy, class three), retrying a few times; a refusal because the remote moved stops it.
6. Only then cleans up: removes the branch's worktree unless it holds work of its own (changed tracked files, unknown files, data the main checkout lacks, all named), deletes the branch with the refusing form, and deletes the remote branch.
Run again after a stop it continues where it stopped. One merge at a time (a lock). Distinct exit codes: done, refused with nothing changed, conflicts, suite failed, not pushed, merged but cleanup left. A queue script stops at the first non-done.

Practicalities:
- Before a chain, bring main into each branch beforehand (or trial-merge) so conflicts surface in the branch's own worktree.
- Run the queue only when load is under the limit; do not merge while a long nightly suite runs.
- Before a push, list commits main has beyond the remote.
- Untracked files are fine; modified tracked files are not.

## Registry semantics
A registry of experiments is a versioned file: a definition is committed before its data comes in; closing writes the outcome beside it. State the machine learns over time (readouts, wake items) lives in an untracked state file so merges never meet it. Writers hold a lock on the registry folder. From a worktree, the registry is the worktree's (what its branch merges) and the state is the main checkout's. A record has a status: planned, running, closed, cancelled.
