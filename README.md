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

Two fork-authored skills complete the set (28 total): `setup-engineering-skills` configures a repository for the others (tracker, labels, domain-docs variant, writing-standard block), and `update-engineering-skills` refreshes the installed skills through the skills CLI and migrates the managed configuration, preserving everything custom under `docs/`.

## Import log

Derivation from upstream is one-directional: Matt Pocock's updates are imported here periodically. Every deliberate divergence from upstream is recorded in this log with its reason, and every skill edit appends an entry, so each import stays a deliberate act.

- 2026-11-12: Repository created as a one-directional derivation of mattpocock/skills, imported at upstream commit 4ae2317ddb22b046e26455e4f9c9b3aaaf62212d. Upstream updates are imported deliberately; nothing is contributed back.
- 2026-11-12: All 26 carried skills copied verbatim; skill names kept from upstream so upstream changes stay visible during imports. The only skill not carried is setup-matt-pocock-skills: it is superseded by setup-engineering-skills, a replacement configured for this repository's docs/ layout, currently being built (see the tracker under docs/tasks/).
- 2026-11-12: Upstream layout `skills/<category>/<name>/` flattened to `skills/<name>/`, and upstream's category READMEs not carried. The skills CLI discovers skills in `skills/` up to three levels deep, so the flat layout installs the same way upstream's catalog layout does.
- 2026-11-12: Router renamed and rewritten: upstream's `ask-matt` is now `ask-skills`, the only renamed skill. Same orientation role, rewritten for this repository's flow: the map routes through the tracker at `docs/tasks/` (feature specs and tickets), promotion close-out by distillation into `docs/specs/`, `docs/adr/`, and `docs/glossary.md`, the rejected-request KB at `docs/out-of-scope/`, research at `docs/research/`, and the `setup-engineering-skills` / `update-engineering-skills` precondition pair. References to a root GLOSSARY.md and `.scratch/` are gone; PHASE-BOUNDARIES.md carried unchanged.
- 2026-11-12: domain-modeling: the file structure section defers to `docs/agents/domain.md` (defaults: `docs/glossary.md` and `docs/adr/`; multi-context variant: `docs/glossary-map.md` with per-context glossaries at `src/<context>/docs/glossary.md`) instead of inferring from a root GLOSSARY.md or GLOSSARY-MAP.md. GLOSSARY-FORMAT.md carries the same paths and deference; ADR-FORMAT.md points multi-context ADR locations at `docs/agents/domain.md`.
- 2026-11-12: to-tickets: the local tracker path moved from `.scratch/<feature-slug>/issues/` to the issue-tracker config's path, defaulting to `docs/tasks/<feature-slug>/issues/`, so tickets are committed and follow their branch.
- 2026-11-12: triage: the rejected-request knowledge base moved from `.out-of-scope/` to `docs/out-of-scope/` so rejections are tracked by git instead of sitting in a gitignored dot-directory; OUT-OF-SCOPE.md and AGENT-BRIEF.md updated to match.
- 2026-11-12: research: findings save to `docs/research/<topic>.md`, one file per topic and dated, instead of wherever the agent guesses; they are point-in-time.
- 2026-11-12: tdd: the glossary line reads the project's glossary at the location `docs/agents/domain.md` describes, not a root GLOSSARY.md.
- 2026-11-12: improve-codebase-architecture: the glossary path follows `docs/agents/domain.md`, ADRs stay at `docs/adr/`, and a matching subsystem spec at `docs/specs/` is orientation input for the scan.
- 2026-11-12: setup-engineering-skills added, superseding upstream's setup-matt-pocock-skills (not carried). Same prompt-driven process (explore, present, confirm, write), rebranded and restructured for this fork's layout: the local markdown tracker at `docs/tasks/<feature>/` is the recommended default (upstream proposed GitHub first), the local tracker seed is rewritten for the committed `docs/tasks/` layout with wayfinding operations, a freeform seed template is added where upstream wrote from scratch, `docs/agents/domain.md` is emitted in a single-context and a multi-context variant with only the chosen one written (and both carry the precedence rule and the check-the-tracker consumer step), the triage-labels table's upstream-branded column header becomes "Canonical role", and setup writes a versioned, delimited writing-standard block (v1) into the steering file alongside the agent-skills block. The GitHub and GitLab tracker seeds are carried from upstream byte-identical.
- 2026-11-12: Six carried skills' pointers updated from the superseded `/setup-matt-pocock-skills` to `/setup-engineering-skills`: to-tickets (2 sites), to-spec, implement-spec, code-review, triage, wayfinder. No other changes to these files.
- 2026-11-12: implement: promotion close-out added after the code-review and commit steps, making promotion part of done. The finished feature distills into the durable docs: behavior into the matching subsystem spec at `docs/specs/<subsystem>.md` (created when none covers the subsystem, kebab-case slug from the glossary term, four sections: Purpose, Behavior, Interfaces and contracts, Decisions in force; no feature slugs or proposed-work marks), hard-to-reverse decisions offered as ADRs at `docs/adr/`, new terms into the glossary at the location `docs/agents/domain.md` describes (`docs/glossary.md` by default), tracker entries resolved the way the issue tracker closes work, and the feature directory removed last with its essence in the durable docs and the rest in git history. The step is done only when all five outcomes are checkable. Upstream has no promotion step; this carries the governance model in ADR 0002.
- 2026-11-12: implement-spec: the same promotion close-out as implement, added as a step after the draft-PR and ticket-resolution step and before worktree cleanup, committing the promotion edits to the integration branch so the PR or branch carries behavior and promotion together.
- 2026-11-12: code-review: the spec-source search names the tracker's feature spec (`docs/tasks/<feature>/spec.md` by default) instead of a generic `docs/`/`specs/`/`.scratch/` sweep, and adds `docs/specs/<subsystem>.md` as the source for promoted changes whose feature directory promotion removed. The Spec sub-agent also receives the paths of the `docs/specs/` subsystem specs whose behavior the diff touches, and its brief gains the promotion-consistency check: a diff changing behavior a subsystem spec describes must carry its promotion, meaning the spec updated in the same change.
- 2026-11-12: to-spec: cross-feature dependency rule added. Exploration checks the tracker's open features (a feature whose directory still sits under the tracker is tracked but unpromoted); the spec template gains a Cross-feature dependencies section naming each dependency by its tracker path, what this feature needs from it, and what happens if it does not land; and ticketing wires each listed dependency as a ticket blocking edge, referenced across feature directories by tracker path per the tracker doc. The publish target now names the tracker path the issue-tracker config defines, `docs/tasks/<feature-slug>/spec.md` by default, replacing "the project issue tracker".
- 2026-11-13: update-engineering-skills added, the maintenance half of the setup/update pair; upstream has no counterpart. The skill implements no installer of its own: file refresh delegates to the skills CLI (`npx skills check`, then `npx skills update`, flagging "has local changes" skills before the update overwrites their hand edits), and the skill's own instructions are migration of the managed artifacts (`docs/agents/*.md` and the steering file's writing-standard block): inventory and classify each artifact against the latest seed templates, read from the installed `setup-engineering-skills` folder the CLI refreshed in the same run (a missing sibling folder stops the run rather than regenerating from memory, and the pair ships together); a migration plan and explicit confirmation before any write; regeneration byte-identical to the seeds; the freeform tracker file never regenerated, since its workflow text is the user's own words; everything unmanaged under `docs/` preserved; and a custom file found at a managed path raised as a conflict instead of overwritten.
- 2026-11-13: update-engineering-skills: the preservation guarantee names the steering file's two managed blocks (the `## Agent skills` block and the writing-standard block), matching the conflict resolution that extends the agent-skills block; ADR 0003 records the same ownership line.
- 2026-11-13: to-spec: a spaced hyphen doing dash work on the rewritten publish-target line replaced with a semicolon (the writing standard's dash rule).
- 2026-11-13: ask-skills: two British spellings normalized to American ("behaviour" to "behavior"), matching the fork's spelling norm.

## License

This repository is MIT licensed; see [LICENSE](LICENSE). The copyright line credits Matt Pocock, whose skill set this repository adapts.
