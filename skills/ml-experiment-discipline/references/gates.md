# Promotion gates

Contents: why sequential; a simple form; choosing the design; choosing the margin; many candidates against one incumbent; the number at the crossing is not the effect; practicalities.

## Why sequential
A fixed-size test forces you to wait for the full sample even when the answer is clear, and tempts you to peek. A sequential test lets you look after every batch without breaking the error rates, provided the boundaries were built for those looks. A fixed-size test read at every look is not that.

## A simple form
- Each paired comparison gives a win, a loss or a tie for the candidate.
- Keep the log likelihood ratio of "candidate is better by the target margin" against "candidate is equal or worse". Update it per outcome.
- Upper boundary from the false-promote rate, lower boundary from the false-drop rate.
- Verdicts: promote, drop, keep playing.
- Set a maximum sample. At the maximum with no crossing, the pre-registered rule applies (typically drop, because a gain too small to detect is too small to pay for).

## Choosing the design
| Situation | Design | Cost |
|---|---|---|
| Maximum sample known, looks in batches | Group sequential with an alpha-spending function; look schedule in the record | Most power; looks follow the plan (spending handles a missed or extra look as long as look times are not chosen after seeing the data; choose the spending function before the start, since it sets how much error is available early) |
| No horizon, or someone watches continuously | Always-valid inference (a mixture test or a confidence sequence) | Valid at any look; less power |
| Two stated hypotheses on win/loss outcomes | A sequential probability ratio test | Fast for clear cases; a maximum sample truncates it, so its nominal rates become approximate |

- The record names the design and holds the look schedule.
- When rounds are paired (same seed, sides swapped), the pair is the unit of the statistic, not the single game.
- A futility stop (conditional power below a stated level) may only drop, never promote.
- Before first use, and after any change, simulate the gate at its registered settings under "no difference" and under the target effect, with the real look schedule and the maximum sample. Record the measured false-promote and promote rates; the labels on the boundaries are a design intent until then.

## Choosing the margin
The smallest improvement that would change what you do. Smaller margins need far more samples; choose by cost, not by hope.

## Many candidates against one incumbent
Decide which error you control over the stream, and write it in the record:
- Per candidate (each at alpha): no promise about the family. With twenty candidates and no real improvement, one false promotion is more likely than not.
- Family-wise: when the number of candidates K is fixed in advance, testing each at alpha/K is the simplest choice and holds the family at alpha. For an open-ended stream, the k-th candidate is tested at alpha_k, with the alpha_k summing to at most alpha over the whole stream (for example alpha times 6 over pi squared k squared; this falls fast, to roughly 8e-5 at the twentieth candidate for alpha 0.05, so a late candidate needs a very strong result). Testing the k-th at alpha/k is not enough: those sum to alpha times (1 + 1/2 + ... + 1/K), about 3.6 alpha at twenty candidates.
- False discovery rate: an online procedure that spends an error budget on each test and earns some back on each promotion; the usual choice when candidates keep coming and a canary catches a rare false promotion.

The family restarts only at a rule written in advance (a new incumbent), never because the bar became uncomfortable.

## The number at the crossing is not the effect
A candidate passes when its quality and its luck together cross the line, so the estimate at a pass is biased upward, more for early passes and for the best of many. Report it as the gate's statistic, not as the gain. The gain is measured on rounds played after the promotion (the canary, or a registered follow-up), and that figure replaces the gate's in every later claim. Never add gate margins across successive promotions.

## Practicalities
- Pair by seed and seat or side so luck cancels.
- Count ties explicitly and say how they enter.
- Keep the log of every outcome so the verdict can be recomputed.
- After promotion, keep a canary: re-run the same gate in the background occasionally, to catch drift.
- Never reuse the data that tuned the candidate as the gate's data.
