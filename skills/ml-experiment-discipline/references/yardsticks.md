# Yardsticks, controls and seeds

Contents: A/A test; sample-ratio and filter checks; equal-latency control; seeds; held-out judging; alternating a treatment within one unit; ladders and ratings; window extension; budget baseline.

## A/A test
Run the baseline against a copy of itself through the same harness at the planned sample size. Expect a mean difference near zero and a spread matching theory. Typical bias sources: first-mover or seat advantage, seed reuse across arms, one arm on faster hardware, caches warming for one arm, time-of-day load. One run shows only gross bias: a large difference points at the sources above. It cannot show that a yardstick is clean; for that, calibrate as below. Record the A/A result with the date; stale A/A results do not count after the harness changes.

One A/A run cannot pass or fail a yardstick: with the null known true and a correctly specified fixed-sample test on independent units (continuous statistics; discrete or sequential ones are not uniform), p-values should be spread evenly and fall under 0.05 about 5% of the time, and a single run shows neither. Calibrate with many:
- **Placebo replays.**
  1. Take logged rounds of the incumbent alone and split them into two pseudo-arms at the unit the real design assigns (lane, time slice, seed pair, player), never round by round: splitting finer than the real assignment hides the clustering and makes the false-promote rate look better than it is.
  2. Keep the time order and the real look schedule, and run the full reader on each split with its stopping rule.
  3. The number of replays sets the precision: at a 5% rate, 300 replays give about plus or minus 2.5 points and about 2,000 give about plus or minus 1. Use enough for the precision you need, or report the interval beside the rate.
  4. The share that crosses the promote bound must match the registered false-promote rate within its binomial interval, and the statistics must look like the null distribution.
  5. Include the looks: a test can end with uniform final p-values and still cross the line at some look far more often than its label.
  6. The interval reflects the number of replays only. Every replay reuses one log, so repeat on logs from different periods before trusting the rate.
  7. Replays are cheap: rerun them after every change to the reader.
- **Live probes.** A copy served through the real pipeline catches what replays cannot (unequal serving, seat or lane effects, time of day). Keep a running count of promotions among probes against the count expected.

## Sample-ratio and filter checks
Before any verdict, compare what each arm contributed with what the design gave it: decided rounds per arm and per stratum (seat, arena, lane, time slice). A chi-square test of the counts (an exact binomial test for small strata, where expected counts are low) against the design ratio at a strict threshold (p < 0.001) marks the readout invalid, not won or lost; find the cause first. Run this check at fixed looks, or in an always-valid form, as `gates.md` explains; repeating it at every look is itself a repeated test. Do the same for every filter applied after assignment: the void rate and the flagged rate per arm. A filter that removes more of one arm's rounds can create an effect or hide one, even when every removal follows its rule.

## Equal-latency control
If a treatment adds compute per decision, give the control the same wall-clock or the same compute, or report both and say which. Otherwise "better" may mean "slower and more thorough".

## Seeds
- A list of seeds per experiment, saved in the record.
- Paired arms use the same seeds.
- A separate held-out seed range for gates, never used for tuning.
- Reduce noise by design before adding sample:
  1. Pair or stratify on what you can fix (seed, seat, opponent, time slice).
  2. Adjust for a covariate measured before the round or the window; the same metric in a pre-period is often a strong one. Register the adjustment in advance, and never choose it after seeing the effect.
  3. Estimate the adjustment's coefficient from the data instead of assuming 1, and first measure the correlation it relies on: a pairing that correlates near zero buys nothing, and a plain before-after difference adds noise when the correlation is under about one half (when the before and after variances are about equal).
  4. When no covariate helps, add seeds.
- A training seed is a unit. A gate pass is a claim about one checkpoint. A claim about a recipe (data, settings, method) needs several training seeds an arm (three is a floor; more when a seed alone moves the result as much as the effect), reported with their spread. Sibling seeds of one recipe share a verdict to a degree: estimate the recipe's effect across its seeds before spending a full gate on each new sibling.

## Held-out judging
The judge (opponent set, scenarios, metric) must be independent of training. If the judge itself is a model, version it and keep a fixed reference version so the scale does not move under you.

A judge consulted for every candidate, whose verdicts steer what is trained next, slowly becomes part of the training loop. Keep three tiers and say which one a claim rests on: an inner yardstick used freely while building; the gate, used once per candidate under its registered rule; an outer yardstick the loop never selects on (fresh seeds and opponents, another build of the environment, people), read at fixed intervals. If the gate's ranking and the outer one drift apart, refresh the gate's fixed parts and re-judge the incumbent.

A benchmark and a model judge need checks of their own:
- Before a benchmark chooses anything, compute the smallest difference it can see from its items, repeats and noise. If that is larger than the differences you expect, it cannot choose.
- Run the strongest available player first. Its failures are mostly the benchmark's: remove broken items, with reasons, before any candidate plays.
- A model judge has noise: judge the same items twice and report the spread. Before swapping in a cheaper judge, measure how often it reverses the costly one's pairs.
- A judge that is also the teacher shares the teacher's taste: keep an objective check beside it and report whether both moved the same way.

## Alternating a treatment within one unit
When units are few and differ a lot, let each be its own control: alternate treatment and control in blocks, draw the order per pair before the start, and judge by the design's own randomisation test.

Carry-over is the weak point. In training, what was learned stays, and it also changes how fast the next block learns, so comparing rates of change within blocks does not remove carry-over (the rate depends on the level already reached). It only protects against slow drifts shared by both conditions.
1. Use long blocks and drop the start of each block as a washout.
2. Write the assumed carry-over in the record, with the washout length chosen so the design can test it.
3. Count the pairs: a sign-flip test over n pairs has a smallest two-sided p of 2/2^n, so at least 6 pairs are needed to reach 5%.
4. With learning that accumulates this design gives weak evidence: prefer random assignment across units and more training seeds when you can.

## Ladders and ratings
- Fit ratings jointly and show each with its interval and round count.
- A rating assumes strength is transitive. Show the pairwise table beside it and look for cycles (A beats B beats C beats A, each beyond its interval); where they exist the table is the result and the single number misleads.
- Ratings depend on who is in the pool. Many near-copies of one player (seeds of one recipe) or many weak players move everyone's rating: rate one representative or weight the pool, and keep fixed anchors.

## Window extension
If lanes stood still (outage, pause, quota stop), extend the close by the same duration. Record: reason, start of stall, end of stall, new close. Nothing else extends a window, except the one extension to the maximum sample that the record already names. This is the only place the rule is stated in full; other pages point here.

## Budget baseline
Before the first run, record solo speed and the others' normal load. Later, compare against this baseline when deciding whether to step down.
