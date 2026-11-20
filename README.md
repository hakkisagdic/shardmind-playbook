# shardmind-playbook

A Claude Code plugin that packages the **approach** used to run a long machine-learning project with one fully autonomous lead agent and a human who mostly reviews.

## Principle

The approach is public; the tooling stays private. This plugin holds method only: skills, slash commands and file templates. It contains no project code, no credentials and no data. You bring your own scripts, and the skills tell you which jobs belong in scripts.

## Install

```
/plugin marketplace add hakkisagdic/shardmind-playbook
/plugin install shardmind-playbook@shardmind-playbook
```

## What is inside

- `skills/autonomous-project-lead`: how one lead agent runs a long project on little human time. State files, a judgement-only loop, autonomy and escalation classes, delegation to narrow roles, a merge queue, shared-machine etiquette, reporting, resumability and corrections. Details in `references/`, including fifty-seven incident rules (what went wrong, the rule, the reason).
- `skills/ml-experiment-discipline`: pre-registered experiments, window rules, A/A tests, sample-ratio checks, self-play populations, controls, a sequential promotion gate, teacher ladders and distillation, relabelling, roll-out labels, shared compute budgets and honest readouts. Details in `references/`.
- `evals/`: three scenarios per skill (setup, prompt, expected behaviour), to be run with and without the skill. They exist; the baseline has not been measured yet.
- `commands/`: `/status-page`, `/tick`, `/diary`, `/preregister`, `/lesson`.
- `skills/autonomous-project-lead/templates/`: skeletons for the status page, open items, lessons, diary, questions queue and resume note.
- `skills/ml-experiment-discipline/templates/`: an experiment record (JSON) with its decision table.

## The story

ShardMind is a self-play reinforcement-learning project for game bots. It was built and run day and night by agents, with the owner acting as reviewer rather than operator.

The owner read a short status page, answered a queue of questions when they had time, and gave an explicit word for anything public, irreversible or costly. The lead decided and carried on with the rest. The rules here come from that one long project and the shared machine it ran on; treat them as a tested starting point to adapt, not as law.

## License

MIT, see `LICENSE`.
