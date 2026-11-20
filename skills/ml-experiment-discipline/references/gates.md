# Promotion gates

## Why sequential
A fixed-size test forces you to wait for the full sample even when the answer is clear, and tempts you to peek. A sequential test lets you look after every batch without breaking the error rates, because the boundaries already account for the looks.

## A simple form
- Each paired comparison gives a win, a loss or a tie for the candidate.
- Keep the log likelihood ratio of "candidate is better by the target margin" against "candidate is equal or worse". Update it per outcome.
- Upper boundary from the false-promote rate, lower boundary from the false-drop rate.
- Verdicts: promote, drop, keep playing.
- Set a maximum sample. At the maximum with no crossing, the pre-registered rule applies (typically drop, because a gain too small to detect is too small to pay for).

## Choosing the margin
The smallest improvement that would change what you do. Smaller margins need far more samples; choose by cost, not by hope.

## Practicalities
- Pair by seed and seat or side so luck cancels.
- Count ties explicitly and say how they enter.
- Keep the log of every outcome so the verdict can be recomputed.
- After promotion, keep a canary: re-run the same gate in the background occasionally, to catch drift.
- Never reuse the data that tuned the candidate as the gate's data.
