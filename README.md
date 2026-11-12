# chrisdigity/skills

Engineering skills for AI coding agents, forked from [mattpocock/skills](https://github.com/mattpocock/skills) and restructured so the `docs/` directory carries the whole agent workflow (tracker, specs, glossary, ADRs, research, rejected-request knowledge base). The fork is maintained independently: upstream changes are imported deliberately, and nothing is contributed back.

## Install

The skills CLI requires Node.js 22.20 or later. From any project:

```bash
npx skills add chrisdigity/skills
```

Add `-g` to install globally instead of project-local.

## Skills

26 skills carried from upstream, names kept so upstream changes stay visible during imports:

- Engineering: ask-matt, code-review, codebase-design, diagnosing-bugs, domain-modeling, grill-with-docs, implement, implement-spec, improve-codebase-architecture, pr, prototype, research, retro, tdd, to-spec, to-tickets, triage, wayfinder, wizard
- Productivity: grill-me, grilling, handoff, teach, to-questionnaire, wait-what, writing-for-agents

Each skill folder holds its SKILL.md plus its reference files, scripts, and agent metadata, carried unchanged from upstream. Skill descriptions live in each SKILL.md and in upstream's README.

## Import log

Derivation from upstream is one-directional: Matt Pocock's updates are imported into this fork periodically, never contributed back. Every deliberate divergence from upstream is recorded here with its reason, and every skill edit in this fork appends an entry, so each import stays a deliberate act.

- 2026-11-12: Repository created as a one-directional fork of mattpocock/skills, imported at upstream commit 4ae2317ddb22b046e26455e4f9c9b3aaaf62212d. Upstream updates are imported deliberately; nothing is contributed back.
- 2026-11-12: All 26 carried skills copied verbatim; skill names kept from upstream so upstream changes stay visible during imports. The only skill not carried is setup-matt-pocock-skills: it is superseded by setup-engineering-skills, a replacement configured for this fork's docs/ layout, currently being built (see the tracker under docs/tasks/).
- 2026-11-12: Upstream layout `skills/<category>/<name>/` flattened to `skills/<name>/`, and upstream's category READMEs not carried. The skills CLI discovers skills in `skills/` up to three levels deep, so the flat layout installs the same way upstream's catalog layout does.

## License

The carried skills are MIT licensed by Matt Pocock; the LICENSE file carries that license unchanged.
