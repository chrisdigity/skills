# Status: ready-for-agent

# Skills fork: chrisdigity/skills

## Problem statement

I use engineering skills derived from Matt Pocock's set across many of my projects. The conventions those skills assume fight how I work: they put the glossary at the repo root, track issues in a dot-prefixed `.scratch/` directory, and keep a rejected-request knowledge base in `.out-of-scope/`. My repositories gitignore all dot-prefixed paths as a rule, so the tracker and the knowledge base are invisible to git; untracked scratch files persist across branch switches, leaving work misaligned with the checked-out branch. Once a feature lands, its spec lives on as a stale tracker artifact while nothing represents how the system is now supposed to behave. Research findings land wherever the agent guesses. Setup is branded for someone else's repo.

## Solution

A fork, in the one-directional sense: `chrisdigity/skills`, installed into any project with `npx skills add chrisdigity/skills`. It carries the full derived skill set, restructured so the docs/ directory holds the whole agent workflow:

```
docs/
|-- agents/          managed per-topic config the skills read
|-- tasks/           the committed local issue tracker (feature specs + tickets)
|-- specs/           one current-state intent document per subsystem
|-- glossary.md      domain language
|-- adr/             decision records
|-- research/        point-in-time cited findings
`-- out-of-scope/    rejected-request knowledge base
```

Two new skills replace upstream's setup: `setup-engineering-skills` configures a project (tracker, labels, docs variant, writing-standard block); `update-engineering-skills` refreshes skill files through the skills CLI and migrates managed configuration to the latest model, preserving everything custom.

## User stories

1. As a developer, I want to install the skill set into any project with `npx skills add chrisdigity/skills`, so that onboarding a repository is one command.
2. As a developer, I want `setup-engineering-skills` to configure the local tracker at `docs/tasks/<feature>/`, so that issues are committed and follow their branch.
3. As a developer, I want the writing standard present in the steering file on turn one of every session, so that all agent output stays readable to humans without per-session reminders.
4. As a developer, I want `update-engineering-skills` to refresh installed skill files through the skills CLI, so that my projects track the fork without hand copying.
5. As a developer, I want `update-engineering-skills` to regenerate managed artifacts to the latest model after presenting a plan, so that my project configuration stays current.
6. As a developer, I want `update-engineering-skills` to preserve custom directories under `docs/`, so that my own agent workflow extensions survive updates.
7. As a developer, I want feature specs and tickets under `docs/tasks/` committed to git, so that work follows its branch and collaborators see it.
8. As a developer, I want completed features promoted into `docs/specs/<subsystem>.md`, so that current intended behavior always has a durable home.
9. As a developer, I want promotion to be part of done and enforced by code-review, so that specs never drift silently.
10. As an agent, I want a precedence rule telling me which source answers which question (code and tests: what is; subsystem specs: what should be; tracker: what is proposed), so that I never get stuck reconciling sources pointing in different directions.
11. As an agent, I want subsystem specs free of feature slugs and proposed-work marks, so that I can read `docs/specs/` as one coherent current specification.
12. As an agent, I want to check the tracker's open features for a subsystem before designing against its spec, so that I account for proposed work that may change it.
13. As a developer, I want research findings at `docs/research/<topic>.md`, so that point-in-time findings do not float around the repository.
14. As a developer, I want rejected enhancement requests recorded at `docs/out-of-scope/` (one file per concept), so that triage deduplicates instead of re-litigating.
15. As a developer, I want a rejected-request KB entry that passes the ADR bar to graduate to `docs/adr/` and leave the KB, so that the KB holds only product-scope rejections.
16. As a developer, I want the glossary at `docs/glossary.md`, so that domain language lives with the rest of the docs.
17. As a developer, I want a multi-context variant available at setup time (`docs/glossary-map.md` plus `src/<context>/docs/glossary.md`), so that a monorepo can adopt the system without redesign.
18. As a developer, I want only the chosen variant described in my project's `docs/agents/domain.md`, so that unused context does not ship.
19. As a maintainer, I want an import log of deliberate divergences from upstream, so that importing Matt's updates stays a deliberate act.
20. As a maintainer, I want skill names kept from upstream (except the router, `ask-skills`), so that upstream changes stay visible during imports.
21. As a developer, I want cross-feature dependencies surfaced in feature specs and wired as ticket blocking edges, so that building on deferred features is a visible decision rather than an accident.
22. As an agent, I want the writing standard to govern my output in every mode (chat, specs, tickets, ADRs, docs), so that humans can read everything I produce.
23. As a developer, I want an `ask-skills` router mapping the flow through the skills, so that I can find the right skill for any situation without remembering them all.

## Implementation decisions

- Repository: `chrisdigity/skills`; skills live under `skills/<name>/SKILL.md` per the skills CLI discovery rules; installed with `npx skills add chrisdigity/skills` (project-local) or `-g` (global).
- Derivation from upstream (Matt Pocock's originals) is one-directional: his updates are imported into this fork periodically, never contributed back. The README carries an import log listing every deliberate divergence and its reason; every skill edit in this build appends to it. Skill names stay upstream's; the router renames `ask-matt` to `ask-skills`.
- The full upstream set is carried (grilling, grill-me, grill-with-docs, diagnosing-bugs, domain-modeling, to-spec, to-tickets, implement, implement-spec, triage, wayfinder, research, prototype, handoff, retro, tdd, code-review, pr, improve-codebase-architecture, codebase-design, wait-what, teach, to-questionnaire, writing-for-agents, find-skills, wizard), plus `setup-engineering-skills` and `update-engineering-skills`. Upstream's `setup-matt-pocock-skills` is superseded by `setup-engineering-skills` and is not carried. Pruning is deferred to retro evidence.
- Tracker: `docs/tasks/<feature>/`, committed. `spec.md` is the feature spec; tickets are one file per slice at `issues/NN-slug.md` with a `Status:` line; wayfinder maps live at `docs/tasks/<effort>/map.md`. Replaces `.scratch/` everywhere.
- Subsystem specs: `docs/specs/<subsystem>.md`, one file per subsystem, created lazily at first promotion, kebab-case slug taken from the glossary term. Four sections: Purpose, Behavior, Interfaces and contracts, Decisions in force (pointers to ADRs). No file paths, no code snippets beyond the prototype-derived exception, no feature slugs, no lifecycle lists, no proposed-work marks.
- Governance (recorded in this repo's ADR 0002): precedence in plain prose (code and tests show what is; subsystem specs show what should be as of the last promotion; the tracker shows what is proposed; conflicts are surfaced, never silently resolved). Promotion is part of done: `implement` and `implement-spec` close out by distilling completed features (behavior into the matching subsystem spec, hard-to-reverse decisions into ADRs, new terms into the glossary; the rest dies with the feature directory). Code-review's spec axis checks that a diff changing spec-described behavior carries its promotion. No in-flight notes on specs. Consumer rule: check the tracker's open features for a subsystem before designing against its spec. Cross-feature dependencies are surfaced in feature specs and wired as ticket blocking edges.
- Glossary: `docs/glossary.md`. The multi-context variant exists only in setup's templates (`docs/glossary-map.md`, per-context glossaries at `src/<context>/docs/glossary.md`); a project's `docs/agents/domain.md` describes only the variant chosen at setup.
- Research: `docs/research/<topic>.md`, point-in-time and dated; never promoted into a spec without re-verification.
- Rejected-request KB: `docs/out-of-scope/`, one file per concept with a prior-requests list; triage reads it before evaluating new requests; an entry that passes the ADR bar graduates to `docs/adr/` and leaves the KB. Replaces `.out-of-scope/`.
- Writing standard: a versioned, delimited block inside the steering file (`AGENTS.md`, or `CLAUDE.md` where that is the existing file), written by setup and replaced in place by update. Wording changes ship only after an approved change list (exact diffs, reasoning, expected effect on model output, human reviewed).
- `setup-engineering-skills`: configures the issue tracker (local markdown defaulting to `docs/tasks/`; GitHub, GitLab, and freeform templates carried from upstream), triage labels (upstream defaults), the domain-docs variant, the managed artifacts, and the steering-file block.
- `update-engineering-skills`: delegates skill-file refresh to `npx skills check` and `npx skills update`; its own body performs migration: regenerate managed artifacts with a plan and confirmation before writes, preserve unmanaged content under `docs/`, raise conflicts rather than overwrite.
- Managed artifacts contract: `docs/agents/*.md` and the writing-standard block are owned by setup and update; everything else under `docs/` is unmanaged and preserved as-is.
- Edits to carried skills: domain-modeling (file structure defers to `docs/agents/domain.md`), to-tickets (local tracker path), to-spec (cross-feature dependency surfacing), implement and implement-spec (promotion close-out), code-review (spec source search including `docs/specs/`, promotion-consistency check), triage (KB relocation), research (`docs/research/`), improve-codebase-architecture (glossary path; `docs/specs/` as orientation input), tdd (glossary line), plus the rewritten router.
- Dogfood: this repository runs the convention itself; its glossary and ADRs 0001 and 0002 already exist.

## Testing decisions

Skills are prose, so verification is seam-based: observable entry-point runs whose expected file states are checkable by inspection, not unit tests. A good verification here is one command path with a defined expected state; the reference state is the settled convention recorded in this repo's glossary and ADRs.

- Seam A (highest): install-and-setup smoke path. In an empty scratch repository: `npx skills add chrisdigity/skills`, then run `setup-engineering-skills`, then verify the resulting layout against the convention (managed artifacts written, writing-standard block present in the steering file, tracker pointing at `docs/tasks/`, chosen variant in `docs/agents/domain.md`).
- Seam B: update migration path. In the same scratch repository: hand-edit a managed artifact, add a custom subdirectory under `docs/`, run `update-engineering-skills`, and verify plan-and-confirm, regeneration of managed artifacts, and preservation of the unmanaged directory.
- Seam C: promotion loop, dogfooded in this repository as this feature's tickets complete; code-review's promotion check exercised when a diff changes spec-described behavior without carrying promotion.
- Deliberately below the seam: per-skill diffs against upstream (the import log covers divergence visibility) and prose linting of skill files (no tool exists; writing-for-agents plus review by reading is the standard).
- Free integrity check: the skills CLI's `skills-lock.json` content hash verifies installed skill files.

## Out of scope

- Contributing anything upstream to Matt's repository.
- Automated test suites or linters for skill prose.
- Renaming any skill other than the router.
- Pruning the skill set (deferred to retro evidence).
- Writing-standard wording changes (gated on an approved change list; separate task).
- Emitting multi-context configuration outside the setup-time choice.
- GitHub, GitLab, and freeform tracker flows beyond carrying upstream's templates unchanged; local markdown is the default and the flow this build verifies.

## Further notes

- The spec includes layout paths (`docs/tasks/`, `docs/specs/`, and the rest) as a deliberate exception to the no-paths rule: for this feature the layout is the product being specified. Paths into individual skill sources are still excluded.
- Bootstrap wrinkle: this repository's own `docs/agents/issue-tracker.md` does not exist until `setup-engineering-skills` is built; until then this repo's flow follows the settled convention directly.
- The skills CLI requires Node.js 22.20 or later and discovers skills in `skills/` up to three levels deep.
- Publishing to GitHub (repository creation, first push) is a human step outside these tickets.
