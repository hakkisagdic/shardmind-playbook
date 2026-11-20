# A simulator and its reference

Contents: the fidelity check; standing looks; comparing; the front door; seats; counting; saved states; a second implementation; rules.

A fast simulator is a model of the real environment. A policy learns its quirks as readily as the task. Training on a simulator that differs from the real thing gives gains that vanish at deployment, so the simulator is held to account like any other measuring instrument. The reference (the real environment's recorded play) is held to account too.

## The fidelity check
- A fidelity check every deploy must pass. Before any learner starts on a new build, play fixed scripted agents in it and compare with the same agents' recorded play in the real environment, by action type and outcome. Tolerances come from the samples' own errors.
- Measure the check's false-failure rate by comparing the reference with itself at the same size. A check that fails a build against its own reference is noise, not a finding.
- A check run only when someone thinks of it finds faults late. A check every deploy must pass finds them first.

## Two standing looks
For as long as the simulator runs:
- What each actor carries at every episode start (items, stats, state) against its template.
- What the episodes look like beside the real ones: how they end, how long they last, how often each action occurs.

## Compare like with like
- Same policy, same opponent, same rule on both sides before calling a gap.
- Start from the quantities the controller itself counts (steps taken, pauses, requests refused), and only then compare outcomes.
- Inspect both sides before blaming either. Scrutinise the reference run as hard as the simulator; the recording itself may be at fault. Repairing one delay can shift how often a different defect shows up, so re-measure after each fix.

## Use the front door
Interact with the environment only through the interface a real client uses. Shortcuts bypass delays, locks and rejections, and a policy trained through them learns an easier game. Also look in the engine for behaviour that switches on only when a real client is connected.

## Seats are a variable
Randomise who moves first, start positions and any role the engine hands out per episode, then verify that each run ends up balanced across them. A lopsided assignment can go unnoticed for a long time if nothing ever tabulates it.

## Count what happened, not what was sent
Count what the environment carried out, not what the client sent, and drop records whose "done" was never confirmed before labelling them.

## Saved states lose things
A state format can silently drop part of the world (timers, hidden values, queued events).
1. List what the format cannot hold.
2. Refuse states the simulator cannot represent, and log the reason.
3. Measure the loss: how well does a value at a saved state predict how that episode really ended?

## A second implementation of a policy
A faster or ported copy of a policy is held to the original step by step on recorded inputs, with its memory fed back in, and by its decisions, not only its numbers (`worked-examples.md`, section 6, on identical output and fixed arithmetic order).

## Rules are part of the game
A time limit or a scoring rule changes what is learned. Compare a run only with runs under the same rule.
