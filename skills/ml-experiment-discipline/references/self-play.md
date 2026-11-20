# Self-play populations

Contents: opponents; exploiters are an instrument; ratings and the worst case; population-based training; what to save; what to check.

Self-play trains an agent against copies of itself. Played only against its newest self, an agent can circle (beat A, forget how to beat B) or grow a blind spot that nothing in its training ever tests. A population fixes much of that, and gives you instruments to measure what is left.

## Opponents
Train against a population, not only the newest self: past selves, fixed scripted anchors, exploiters. Weight opponents by how hard they are for the learner (for example by the learner's recent loss rate against each), with a floor so none vanishes. Anchors never change; they keep old skills measurable.

## Exploiters are an instrument
An exploiter is a second learner whose only goal is to beat the current main agent, or the whole population. Count how many training steps it needs to reach a chosen win rate and log that count for every generation. A rising count means the main agent has fewer weak spots than before; a falling one means new ones. If an exploiter wins almost immediately, do not celebrate or dismiss it. First confirm that it faced the opponent you meant it to face, because a quiet substitute for a missing opponent produces the same picture (this is rule 38 in `incident-rules.md` of the autonomous-project-lead skill). Restart exploiters from scratch every so often so that they do not settle into a narrow niche. If the count stays flat across generations, there are several remedies; try the cheapest first. Mix in more games between the main agent and stored exploiter checkpoints, widen the exploiter family (older starting points, targets spanning the whole population), derive opponent weights from the equilibrium of the payoff matrix, or split the agent into specialists behind a selector.

## Ratings and the worst case
A rating summarises average strength against a pool. It says nothing about the worst case: a small, cheap adversary can beat a very strong agent that beats strong players easily. Report the worst case found (the best exploiter's win rate, with its count and interval) beside every rating. For what a rating assumes, see "Ladders and ratings" in `yardsticks.md`.

## Population-based training
Copy a better member's weights and settings only when the fitness difference survives a multiple-comparison correction over all the members being compared, and also exceeds a minimum gap, on fitness measured over equal windows. Noisy fitness with greedy copying spreads luck through the population. Log every copy and perturbation. In an experiment on a population, the member is the unit, and no copy crosses the arms.
- Before use, simulate the copy rule at your own rates: how often are equal members copied, and how often is a member a stated gap behind found?
- Rank members by their last measured fitness, not only the freshly measured ones, or a member that was just replaced drops out and the next one is replaced on no evidence.
- Cap how much of the population one lineage may hold.

## What to save
Save every file the population names (snapshots, exploiters), not what a file pattern finds. A population that names missing opponents fails or, worse, substitutes silently.

## What to check
- Is the worst case measured, and does it move over generations?
- Does the gate's judge include an opponent the loop never trained against (`yardsticks.md`, "Held-out judging")?
- Does every copy in a population-based run have a logged, significant reason?
- Does the learner draw roughly even against a snapshot of itself taken just now? If not, the snapshot is loaded or run differently from the live agent.
- On a plateau, split the episodes by how they end before searching settings. If a time limit decides many of them, the policy must be able to see the clock.
- Has every type played every other, in training and at the judge? Mirror-only play trains and measures mirrors.
- Were the sampling shares and floors tuned for today's population re-examined for the larger population it will grow into?
