# How a pre-registration reads

A worked example on an invented scenario, with round illustrative numbers. A team has an agent that plays a card game and wants to know whether it beats club players when they sit across the table from it. The shape is what matters.

## Header
- **Id**: a short slug, unique forever.
- **Question** (the owner's question it feeds): over a session of games, do club players score less than half the points against the agent?
- **Hypothesis**: the mean per-player score share of the club players is below 0.50. Write your prior too, including where you expect to be wrong. Only the pooled figure is the claim; breakdowns are descriptive.
- **Claim sentence**: the exact sentence a win would let you say, with its population and conditions. Nothing broader is said afterwards.

## Unit and metric
- **The unit is the player, not the game.** Games by one person are correlated; treating them as independent makes the interval too narrow. Take each player's share of wins among their eligible games, then average across players, so a single enthusiastic player cannot dominate.
- Interval: bootstrap over players, percentile 95%, analysis code frozen and committed before any data (see the next point when players are few).
- Count the units behind every interval and print the count beside it. With fewer than a few dozen units an arm, a normal interval and a percentile bootstrap can be too narrow: use Student's t with the units' degrees of freedom, or the design's own randomisation test (shuffle or sign-flip the assignment) when assignment was drawn at random. A randomisation test has a floor under its p-value: with three units an arm there are only 20 possible assignments, so the smallest two-sided p is 0.10 and nothing can be significant at 5%; with n sign-flip pairs it is 2/2^n. Compute that floor before writing the record; at least four units an arm (70 assignments, floor about 0.03) or six pairs are needed for a two-sided 5% test. With Student's t on very few units, only a very large effect is detectable; say so in the power line instead of running it.
- Secondary measures (reported, not decisive): the same players against a simple scripted opponent, results by experience level, results by game number (learning, novelty), results against the agent's thinking time.

## Decision table (written before data)
| Verdict | Condition | What happens |
|---|---|---|
| win (agent ahead) | the interval's upper bound for the players' share is below 0.50 | the next product step, named in advance |
| loss (agent behind) | the interval's lower bound is above 0.50 | read the breakdowns for where; the lab benchmark did not transfer |
| null | the interval sits inside 0.45 to 0.55 at the maximum sample | say "even, to within 5 points" and nothing more |
| unclear | none of the above at the minimum sample | extend once to the maximum; still unclear is reported as unclear |

## Power from the variances you have
- Per-game variance starts from the even-split value 0.25, inflated by whatever design effect your own data show. With m games per player, a player's share has within-player variance of that divided by m.
- Between-player variance is unknown if all your data come from scripted opponents. State an assumed spread, a plausible range, and say it is the one assumption.
- Build a small table: players needed for 80% power at three assumed spreads and three true shares, and the standard error at the minimum and maximum sample. Illustrative outcome: a minimum of 40 players with 6 games each and a maximum of 90.
- One permitted peek at spread: once the first dozen players are in, re-estimate only the between-player spread (no scores) and, if it is larger, raise the maximum. No looks at the effect before the minimum.

## Pre-test before any person plays
Run the agent against the scripted opponent through the same pipeline. It must reproduce the lab figure within about two standard errors and meet a response-time bound. If not, fix the setup first.

## Stopping rule and protocol
- Fix the minimum, the maximum, the one extension and the closing date in the record. Nothing else stops or extends the study.
- Pre-generate the schedule of opponents and game order, seeded, and commit it. Participants do not know which arm they are in until their set ends. Balance order and position with a constrained random table.
- Freeze versions (agent build, game engine, settings) in the record and in every game log. Name the build and the runner, not only the model; keep one model under both as the runner's control. Any change closes the segment; analyse segments separately.
- Define void games from the system's own log, independent of who was ahead (for example, a participant disconnects early). Report voids; never count them as agent wins.
- Say in advance how the agent's response time is handled: recorded and disclosed, not filtered afterwards.

## People: consent and data
- Informed consent before the first game, versioned with the rules; a separate, default-off consent for any reuse for training.
- Collect the minimum: a pseudonym, an experience level, game records. No names, contact details or free text.
- Keep the key that links pseudonyms to people apart from the results, with a short retention, and destroy it after the study so individuals can no longer be found.
- Participants can stop at any moment and withdraw their data until the analysis is locked; removal is by person, never by result.
- An automatic check confirms that the data files hold no identifiers.
- Have the legal side reviewed before the first session.

## Say plainly what is likeliest to miss
Recruitment. Plan for a low completion rate and say how many sign-ups that implies.
