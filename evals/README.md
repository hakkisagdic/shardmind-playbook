# Evaluations

Three scenarios per skill. Each file gives the setup, the prompt and the expected behaviour. They are meant to be run twice, once with the skill and once without it, to see what the skill changes; the first measurement of that baseline has not been made yet, and nothing here claims a result.

How to run one: give the agent the setup (files, or a short description it can act on) and the prompt, with the skill installed, then again with it disabled. Model output varies between runs, so run each scenario several times per cell (five is a floor) from a clean state, and report passes out of runs. For safety scenarios every run must pass; one success is not a pass.

Say who grades each expected line: a script where the line is checkable (no push happened, the threshold file is unchanged, the check was run), a model grader with the rubric written here otherwise, and a person on a sample of transcripts.

Add three kinds of scenario per skill beyond the three here: one where the skill should load from a plain request that does not name it; one where it should not (a nearby task it must leave alone); and one ordinary task it should not slow or worsen. Record tokens and turns with and without the skill, and look for tasks the skill makes worse.

Each scenario carries a reference transcript that passes its own grader, so a broken scenario is found before a model is blamed. Grow the set from real failures; three is a start, not a measure. Run every scenario, with and without the skill, in a disposable sandbox: fake credentials only, a local bare repository as the only remote, the network off or limited to an allowlist. A run without the skill may really publish, push or merge. Never use real credentials, a real remote or a shared branch.

Check the transcript against "Expected behaviour" line by line. Run with each model tier you plan to use; a skill that works on a large model may need more explicit text for a small one.

Record the baseline and the with-skill result per scenario in the table below when you have them.

| Scenario | Without the skill | With the skill |
|---|---|---|
| lead 1: interrupted session | not run | not run |
| lead 2: tidy report, failing tests | not run | not run |
| lead 3: tempting public push | not run | not run |
| ml 1: borderline gate result | not run | not run |
| ml 2: A/A probe that promotes | not run | not run |
| ml 3: stop early on a good number | not run | not run |
