# Evaluations

Three scenarios per skill. Each file gives the setup, the prompt and the expected behaviour. They are meant to be run twice, once with the skill and once without it, to see what the skill changes; the first measurement of that baseline has not been made yet, and nothing here claims a result.

How to run one: give the agent the setup (files, or a short description it can act on) and the prompt, with the skill installed, then again with it disabled. Run every scenario, with and without the skill, in a disposable sandbox: fake credentials only, a local bare repository as the only remote, the network off or limited to an allowlist. A run without the skill may really publish, push or merge. Never use real credentials, a real remote or a shared branch.

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
