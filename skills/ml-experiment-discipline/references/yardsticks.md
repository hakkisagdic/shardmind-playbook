# Yardsticks, controls and seeds

## A/A test
Run the baseline against a copy of itself through the same harness at the planned sample size. Expect a mean difference near zero and a spread matching theory. Typical bias sources: first-mover or seat advantage, seed reuse across arms, one arm on faster hardware, caches warming for one arm, time-of-day load. Fix and rerun until clean. Record the A/A result with the date; stale A/A results do not count after the harness changes.

## Equal-latency control
If a treatment adds compute per decision, give the control the same wall-clock or the same compute, or report both and say which. Otherwise "better" may mean "slower and more thorough".

## Seeds
- A list of seeds per experiment, saved in the record.
- Paired arms use the same seeds.
- A separate held-out seed range for gates, never used for tuning.
- When noise matters more than the effect, add seeds rather than trying cleverer analysis.

## Held-out judging
The judge (opponent set, scenarios, metric) must be independent of training. If the judge itself is a model, version it and keep a fixed reference version so the scale does not move under you.

## Window extension
If lanes stood still (outage, pause, quota stop), extend the close by the same duration. Record: reason, start of stall, end of stall, new close. Nothing else extends a window.

## Budget baseline
Before the first run, record solo speed and the others' normal load. Later, compare against this baseline when deciding whether to step down.
