# Labels, teachers and roll-outs

Contents: label record fields; ladder; distillation checks; relabelling loop; roll-out procedure; thresholds; on the threshold.

## Label record fields
Source (which teacher or process), level (rung on the ladder), cost, date, version of the teacher, state id, the label, confidence or margin, and a flag for ambiguity.

## Ladder
Rung 0: rules and heuristics (fast, narrow, never tired). Rung 1: a small learned model. Rung 2: search or a large model. Rung 3: counterfactual roll-outs with many seeds. Higher rungs cost more per label and are used where lower rungs disagree with each other, are uncertain, or are known weak.

Qualify a teacher before buying labels at scale: on a small probe set whose better and worse options are known from outcomes, does it choose the better more often? A teacher that fails the probe makes the student worse, however strong it is elsewhere. Say of every threshold whether it ranks teachers or admits them; a bar meant to choose among teachers, read as a floor, can cost weeks.

## Distillation checks
- Student agreement with teacher on held-out states, per situation type, not just overall.
- Student value in play, not only agreement: a student can match labels and still lose.
- Gap analysis: where student and teacher disagree and the teacher is right.

## Relabelling loop
1. Student plays; save the states it reaches.
2. Teacher labels those states (at the cheapest adequate rung).
3. Train on old plus new, with replay weights.
4. Gate the result (see gates). Repeat.
Relabel only the slice a change touches; full relabelling costs a lot and hides which change helped.
Before a round, measure the share of the student's states where the teacher keeps the student's choice. When it is high, the round will tie, and only a stronger teacher moves the student. A round whose labels come from a different teacher is a new-teacher experiment, not a correction: measure the changed share and its direction, keep the student's earlier set as it was, and test the teacher on the gate's own opponent, not only on the student's positions.

## Roll-out procedure
- Fork the saved state at the decision into each legal alternative.
- Play each alternative to the end N times, same seeds for all alternatives. Say how the states were sampled: states chosen by what happened later are good for finding mistakes and biased for averages.
- Value is the mean outcome; regret is best minus chosen.
- Pick N from the noise: enough that the margin you care about is several standard errors.
- Log the continuation policy and its version.
- Choose the continuation policy on purpose. Continuing with the teacher values each option as if the teacher took over: it cannot teach past the teacher's weak spots. Continuing with the student's own policy values options as the student will really play them: improvement on itself. A per-state mixture keeps both. Log which one each label used.

## Thresholds
Write before looking: the margin required between best and runner-up, the agreement required across independent seed batches, and what happens in between (ambiguous). Keep the share of ambiguous states visible; if it is large, the decision may not matter or the roll-outs are too noisy. When many states are flagged "beyond the noise", set a chance baseline first: with K options and S states, how many would pass by luck alone? If the flagged share is about what chance gives, there is no finding yet.

## On the threshold
A number equal to or within noise of the threshold is borderline. Report it with its interval. Apply the borderline rule you wrote. If you did not write one, the safe default is no promotion and a fresh pre-registered rerun, not a threshold change.
