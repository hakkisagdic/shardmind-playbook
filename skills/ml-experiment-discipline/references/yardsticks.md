# Yardsticks, controls and seeds

Contents: A/A test; sample-ratio and filter checks; equal-latency control; seeds; held-out judging; alternating a treatment within one unit; ladders and ratings; window extension; budget baseline.

## A/A test
Run the baseline against a copy of itself through the same harness at the planned sample size. Expect a mean difference near zero and a spread matching theory. Typical bias sources: first-mover or seat advantage, seed reuse across arms, one arm on faster hardware, caches warming for one arm, time-of-day load. Fix and rerun until clean. Record the A/A result with the date; stale A/A results do not count after the harness changes.

One A/A run cannot pass or fail a yardstick: with the null known true and a correctly specified fixed-sample test on independent units (continuous statistics; discrete or sequential ones are not uniform), p-values should be spread evenly and fall under 0.05 about 5% of the time, and a single run shows neither. Calibrate with many:
- **Placebo replays.** Take logged rounds of the incumbent alone and split them into two pseudo-arms, at the unit the real design assigns (lane, time slice, seed pair, player), never round by round: splitting finer than the real assignment hides the clustering and makes the false-promote rate look better than it is. Keep the time order and the real look schedule, and run the full reader on each split with its stopping rule. The number of replays sets the precision: at a 5% rate, 300 replays give about plus or minus 2.5 points and about 2,000 give about plus or minus 1, so either use enough for the precision you need or report the interval beside the rate. The share that crosses the promote bound must match the registered false-promote rate within its binomial interval, and the statistics must look like the null distribution. Include the looks: a test can end with uniform final p-values and still cross the line at some look far more often than its label. Replays are cheap: rerun them after every change to the reader.
- **Live probes.** A copy served through the real pipeline catches what replays cannot (unequal serving, seat or lane effects, time of day). Keep a running count of promotions among probes against the count expected.

## Sample-ratio and filter checks
Before any verdict, compare what each arm contributed with what the design gave it: decided rounds per arm and per stratum (seat, arena, lane, time slice). A chi-square test of the counts (an exact binomial test for small strata, where expected counts are low) against the design ratio at a strict threshold (p < 0.001) marks the readout invalid, not won or lost; find the cause first. Do the same for every filter applied after assignment: the void rate and the flagged rate per arm. A filter that removes more of one arm's rounds can create an effect or hide one, even when every removal follows its rule.

## Equal-latency control
If a treatment adds compute per decision, give the control the same wall-clock or the same compute, or report both and say which. Otherwise "better" may mean "slower and more thorough".

## Seeds
- A list of seeds per experiment, saved in the record.
- Paired arms use the same seeds.
- A separate held-out seed range for gates, never used for tuning.
- Reduce noise by design before adding sample: pair or stratify on what you can fix (seed, seat, opponent, time slice), and adjust for a covariate measured before the round or the window (the same metric in a pre-period is usually the best). Register the adjustment in advance, estimate its coefficient from the data instead of assuming 1, and first measure the correlation it relies on: a pairing that correlates near zero buys nothing, and a plain before-after difference adds noise when the correlation is under about one half (when the before and after variances are about equal). Never choose the adjustment after seeing the effect. When no covariate helps, add seeds.
- A training seed is a unit. A gate pass is a claim about one checkpoint. A claim about a recipe (data, settings, method) needs several training seeds an arm (three is a floor; more when a seed alone moves the result as much as the effect), reported with their spread. Sibling seeds of one recipe share a verdict to a degree: estimate the recipe's effect across its seeds before spending a full gate on each new sibling.

## Held-out judging
The judge (opponent set, scenarios, metric) must be independent of training. If the judge itself is a model, version it and keep a fixed reference version so the scale does not move under you.

A judge consulted for every candidate, whose verdicts steer what is trained next, slowly becomes part of the training loop. Keep three tiers and say which one a claim rests on: an inner yardstick used freely while building; the gate, used once per candidate under its registered rule; an outer yardstick the loop never selects on (fresh seeds and opponents, another build of the environment, people), read at fixed intervals. If the gate's ranking and the outer one drift apart, refresh the gate's fixed parts and re-judge the incumbent.

## Alternating a treatment within one unit
When units are few and differ a lot, let each be its own control: alternate treatment and control in blocks, draw the order per pair before the start, and judge by the design's own randomisation test. In training, effects carry over: what was learned stays, and it also changes how fast the next block learns, so comparing rates of change within blocks does not remove carry-over (the rate depends on the level already reached). It only protects against slow drifts shared by both conditions. Use long blocks, drop the start of each block as a washout, and write the assumed carry-over in the record, with the washout length chosen so the design can test it. With learning that accumulates, this design gives weak evidence: prefer random assignment across units and more training seeds when you can. A sign-flip test over n pairs has a smallest two-sided p of 2/2^n, so at least 6 pairs are needed to reach 5%.

## Ladders and ratings
- Fit ratings jointly and show each with its interval and round count.
- A rating assumes strength is transitive. Show the pairwise table beside it and look for cycles (A beats B beats C beats A, each beyond its interval); where they exist the table is the result and the single number misleads.
- Ratings depend on who is in the pool. Many near-copies of one player (seeds of one recipe) or many weak players move everyone's rating: rate one representative or weight the pool, and keep fixed anchors.

## Window extension
If lanes stood still (outage, pause, quota stop), extend the close by the same duration. Record: reason, start of stall, end of stall, new close. Nothing else extends a window.

## Budget baseline
Before the first run, record solo speed and the others' normal load. Later, compare against this baseline when deciding whether to step down.
