---
description: Open an experiment with its rule written before any data
argument-hint: "<short experiment idea>"
---
Open a pre-registered experiment for: $ARGUMENTS. Use the ml-experiment-discipline skill and `templates/experiment.json` in the ml-experiment-discipline skill folder.

Fill in: id, hypothesis (falsifiable, directional), measure, control (and equal-latency or A/A needs), seeds, minimum sample, read rule (test and thresholds), the decision table (win, loss, null, unclear, borderline) with next steps, void rules, frozen versions, budget floor, window and the planned interim readout if any.
Save the record and commit or register it before running anything. Then print the record's id and the exact line that states the read rule. Do not look at any data for this experiment before the record is saved.
