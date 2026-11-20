# Worked examples

Invented scenarios with round, purely illustrative numbers. They show the shape of the reasoning, not anyone's results. The running scenario: a team trains agents for a card game and wants to know whether a new version should replace the current one (the incumbent).

## 1. Extending a window by exactly the time lost
An experiment has a 48 hour window. On day one a power cut stops all workers for 6 hours.
- Measure the stall from a log the system wrote (last heartbeat before, first after), not from memory.
- New close = old close + 6 hours. Write down: reason, stall start, stall end, new close.
- Anything that straddled the stall is discarded and replayed, not counted.
- No other reason extends a window. "The result is close and we want more data" is either the extension the record already allows, or a new experiment.
- Scope every reader to the window: a verdict is computed only from data inside it, so an earlier run of the same candidate cannot leak in.

## 2. What an A/A test can catch
The gate compares candidate against incumbent. To test the gate itself, enter a copy of the incumbent as the candidate. A fair gate should say "no difference" almost always.
Suppose it instead promotes the copy. Look for the usual suspects:
- **Mixed conditions**: the two sides' games came from different parts of the day or different machines. Compare like with like (same time slices, weighted the same).
- **Unequal service**: the copy answered faster than the incumbent, and speed helps. Record each side's response time; compare only games served within a tolerance, or give both sides the same queue.
- **Repeated looks**: checking after every batch with a fixed-size test raises the false-positive rate well above its label.
- **Many candidates**: with many candidates against one incumbent, some will pass by luck. Raise the bar with the number tried.
- **Past promotions**: re-run earlier promotions through the repaired gate. Some may no longer pass; report that plainly.
Keep A/A probes running as registered experiments of kind "A/A": a promotion, or a large statistic in either direction, is the yardstick's bias; a tie is the expected null.

## 3. How a gate decides
- Each paired comparison adds to a running statistic. Two boundaries come from the false-promote and false-drop rates you chose; a maximum sample caps the effort.
- Upper boundary: promote. Lower: drop. Between: keep playing.
- At the maximum sample with no crossing, apply the pre-written rule (usually drop, or one extension).
- Example: the boundaries are set so a candidate that is truly better by 3 points is promoted 80% of the time and an equal one under 5%. Clear losers stop after a fraction of the sample.
- A lead that passes a simple threshold may still be undecided if the control's data are not comparable. Say why.
- A second, independent judge can govern a stage where the first saturates: if the incumbent beats the old benchmark nearly always, wins against it cannot rank new candidates, so rate them head to head instead. Decide in advance which judge governs which stage.
- A scoreboard shows no figure without its sample size and interval.

## 4. A borderline result
Rule written in advance: among states where option A's best move is clear, measure how often option B's best move is the same. At or above a high line keep A; below a low line switch to B; in between keep only the labels on which both agree.
Imagine the result is a hair under the low line.
- Apply the rule as written. Do not nudge the line.
- Report: the value, its standard error, and how repeatable B is with itself on those states (agreement cannot exceed that).
- Mark it a lean, not a verdict. Confirm on a fresh, larger, separate set under identical thresholds before the costly step.
- Keep the main finding apart: a finding that holds under both options is stated as such.
- Claim no downstream benefit (a better trained model) until it is measured.

## 5. Checking labels from a teacher
- Agreement with the teacher is a diagnostic. Strength in real play is the judge: a student can match a teacher closely and still lose.
- A teacher need only be better than the student at the states the student reaches; it need not be perfect.
- A weak offline test with a wide interval is no evidence either way. Report the interval; do not decide on it.
- Break agreement down by situation type, not only overall.
- Before training: check each label's source and version, the number of roll-outs it rests on, and refuse labels whose count differs from the declared one or whose two arms were evaluated unequally.
- Rare, high-value labels are easily lost among many ordinary rows. Weight them (for example by their measured gap, clipped to a range) and register the expectation that a handful of labels will give a tie, with the next step written into the null line.
- A counterfactual label is an outcome; a teacher label is an opinion. Keep them in separate fields.

## 6. Roll-out measurement checks
Before trusting values from simulated continuations:
- Forking a state and replaying with no change reproduces the original run exactly.
- A hand-built state with one known winning option: the tool finds it and reports the known regret.
- A fixed seed yields identical output with one worker or many; a state's output does not depend on other states.
- Forcing the policy's own choice equals leaving it unforced.
- Standard error falls with the square root of the roll-out count: four times as many halves it. Choose N from the margin you care about.
- A faster implementation must produce identical output; keep a hash test.
- Choose the best option on one half of the roll-outs and estimate its value on the other half, to avoid picking the luckiest.

## 7. Enforcing a shared budget
Suppose your training shares a device with a colleague's job.
- Baseline: record their rate alone, then beside each of your consumers in turn for a short fixed window.
- Write down the floor they must keep. If it is breached, step down one consumer at a time and re-measure after each step; stop when the floor holds.
- Run heavy work in a fixed window with a hold flag in a file whose contents say why it is set.
- Cap total lanes, not per experiment. Fewer clean lanes beat many noisy ones.
- Flag runs whose latency exceeds a limit automatically; alarm on the flagged share; do not filter afterwards.

## 8. Null and negative results
A pre-registered comparison that shows no difference is a result and is recorded as one. A shortcut that looked promising and measured slower is recorded with its number and the reason. A cheap early signal that ranks candidates the wrong way earns a line in the lessons.
