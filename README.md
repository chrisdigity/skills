# chrisdigity/skills

Engineering skills for AI coding agents, adapted from [mattpocock/skills](https://github.com/mattpocock/skills) to suit alternate preferences. The `docs/` directory carries the whole agent workflow (tracker, specs, glossary, ADRs, research, rejected-request knowledge base), committed alongside the skills.

## Install

The skills CLI requires Node.js 22.20 or later. From any project:

```bash
npx skills add chrisdigity/skills
```

Add `-g` to install globally instead of project-local.

## Skills

26 skills carried from upstream, names kept so upstream changes stay visible during imports; the one rename is the router, upstream's `ask-matt`, which is `ask-skills` here:

- Engineering: ask-skills, code-review, codebase-design, diagnosing-bugs, domain-modeling, grill-with-docs, implement, implement-spec, improve-codebase-architecture, pr, prototype, research, retro, tdd, to-spec, to-tickets, triage, wayfinder, wizard
- Productivity: grill-me, grilling, handoff, teach, to-questionnaire, wait-what, writing-for-agents

Each skill folder holds its SKILL.md plus its reference files, scripts, and agent metadata, carried unchanged from upstream; the router is the exception, rewritten for this repository's docs/ flow. Skill descriptions live in each SKILL.md and in upstream's README.

## Import log

Derivation from upstream is one-directional: Matt Pocock's updates are imported here periodically. Every deliberate divergence from upstream is recorded in this log with its reason, and every skill edit appends an entry, so each import stays a deliberate act.

- 2026-11-12: Repository created as a one-directional derivation of mattpocock/skills, imported at upstream commit 4ae2317ddb22b046e26455e4f9c9b3aaaf62212d. Upstream updates are imported deliberately; nothing is contributed back.
- 2026-11-12: All 26 carried skills copied verbatim; skill names kept from upstream so upstream changes stay visible during imports. The only skill not carried is setup-matt-pocock-skills: it is superseded by setup-engineering-skills, a replacement configured for this repository's docs/ layout, currently being built (see the tracker under docs/tasks/).
- 2026-11-12: Upstream layout `skills/<category>/<name>/` flattened to `skills/<name>/`, and upstream's category READMEs not carried. The skills CLI discovers skills in `skills/` up to three levels deep, so the flat layout installs the same way upstream's catalog layout does.
- 2026-11-12: Router renamed and rewritten: upstream's `ask-matt` is now `ask-skills`, the only renamed skill. Same orientation role, rewritten for this repository's flow: the map routes through the tracker at `docs/tasks/` (feature specs and tickets), promotion close-out by distillation into `docs/specs/`, `docs/adr/`, and `docs/glossary.md`, the rejected-request KB at `docs/out-of-scope/`, research at `docs/research/`, and the `setup-engineering-skills` / `update-engineering-skills` precondition pair. References to a root GLOSSARY.md and `.scratch/` are gone; PHASE-BOUNDARIES.md carried unchanged.
- 2026-11-12: domain-modeling: the file structure section defers to `docs/agents/domain.md` (defaults: `docs/glossary.md` and `docs/adr/`; multi-context variant: `docs/glossary-map.md` with per-context glossaries at `src/<context>/docs/glossary.md`) instead of inferring from a root GLOSSARY.md or GLOSSARY-MAP.md. GLOSSARY-FORMAT.md carries the same paths and deference; ADR-FORMAT.md points multi-context ADR locations at `docs/agents/domain.md`.
- 2026-11-12: to-tickets: the local tracker path moved from `.scratch/<feature-slug>/issues/` to the issue-tracker config's path, defaulting to `docs/tasks/<feature-slug>/issues/`, so tickets are committed and follow their branch.
- 2026-11-12: triage: the rejected-request knowledge base moved from `.out-of-scope/` to `docs/out-of-scope/` so rejections are tracked by git instead of sitting in a gitignored dot-directory; OUT-OF-SCOPE.md and AGENT-BRIEF.md updated to match.
- 2026-11-12: research: findings save to `docs/research/<topic>.md`, one file per topic, instead of wherever the agent guesses.
- 2026-11-12: tdd: the glossary line reads the project's glossary at the location `docs/agents/domain.md` describes, not a root GLOSSARY.md.
- 2026-11-12: improve-codebase-architecture: the glossary path follows `docs/agents/domain.md`, ADRs stay at `docs/adr/`, and a matching subsystem spec at `docs/specs/` is orientation input for the scan.

## License

This repository is MIT licensed; see [LICENSE](LICENSE). The copyright line credits Matt Pocock, whose skill set this repository adapts.
