# chrisdigity/skills

Engineering skills for AI coding agents, adapted from [mattpocock/skills](https://github.com/mattpocock/skills) to suit alternate preferences. The `docs/` directory carries the whole agent workflow (tracker, specs, glossary, ADRs, research, rejected-request knowledge base), committed alongside the skills.

## Install

The skills CLI requires Node.js 22.20 or later. From any project:

```bash
npx skills add chrisdigity/skills
```

Add `-g` to install globally instead of project-local.

## Skills

27 skills carried from upstream, names kept so upstream changes stay visible during imports; two renames: upstream's `ask-matt` is `ask-skills` here, and upstream's `setup-matt-pocock-skills` is `setup-engineering-skills` here:

- Engineering: ask-skills, code-review, codebase-design, diagnosing-bugs, domain-modeling, grill-with-docs, implement, implement-spec, improve-codebase-architecture, pr, prototype, research, retro, setup-engineering-skills, tdd, to-spec, to-tickets, triage, wayfinder, wizard
- Productivity: grill-me, grilling, handoff, teach, to-questionnaire, wait-what, writing-for-agents

Each skill folder holds its SKILL.md plus its reference files, scripts, and agent metadata. Skill descriptions live in each SKILL.md and in upstream's README.

One fork-authored skill completes the set (28 total): `update-engineering-skills` refreshes the installed skills through the skills CLI and migrates the managed configuration (tracker, labels, domain-docs variant, git policy, writing-standard block), preserving everything custom under `docs/`.

## Divergences from upstream

Derivation from upstream is one-directional: Matt's updates are imported here periodically, never the reverse. Every deliberate divergence and its reason is listed in [docs/upstream-divergences.md](docs/upstream-divergences.md), one entry per live divergence, maintained in place: a skill edit updates its divergence's entry or adds one, which is what keeps each import a deliberate act.

## License

This repository is MIT licensed; see [LICENSE](LICENSE). The copyright line credits Matt Pocock, whose skill set this repository adapts.
