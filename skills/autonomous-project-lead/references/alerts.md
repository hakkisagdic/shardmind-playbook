# Alerts

Rules 2, 4, 8, 55 and 59 in `incident-rules.md` say that alerts must exist, must be tested and must not be repeated. This page says what an alert is and how many a person can carry. Numbers below are round illustrations.

## What an alert is
- An alert is something a named responder must act on within a stated time. Anything else is a line in a digest.
- One record per alert: what it means, what happens if it is ignored, the action, who acts, how fast, and the test that makes it fire (rule 59).
- A warning that has no action is not an alert. Put it in the digest, or delete it.

## A budget per responder
- Give each responder a budget. For the owner: a handful a day, and at night only what cannot wait until morning. For the lead: a rate it can act on within a round.
- Over budget, fix or remove alerts before adding one. Every past failure gets a check, but not every check needs a person: most belong in the digest.
- Control-room standards aim at roughly one or two alarms per operator per ten minutes. The same holds for monitors in general: too many false firings erode trust in the real ones. A person's attention is the limit, not the number of things you can measure.

## Floods
- A flood is many alerts from one cause (a lost network link makes every job look stalled). Group by cause, show the first, and hold back the ones that follow from it.
- Where one failure can lead to several retries or reminders, send one alert for the cause, not one per effect.

## Count weekly
- Alerts raised, and the share that led to an action.
- The ten that rang most. These are the first to fix.
- Alerts standing for more than a day. A standing alert is fixed, silenced with an expiry (rule 55), or deleted.
- Review the records in the same pass: does each still have an action and a responder?

## Fit thresholds to the thing watched
Use the watched thing's own baseline (its normal rate, its normal quiet hours), not one number for all. A threshold set for a busy worker fires all day on an idle one, and the reverse hides a stall.
