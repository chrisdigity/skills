# 03: Location re-skin

What to build: Mechanical edits so no carried skill hardcodes a moved location: domain-modeling (file structure defers to docs/agents/domain.md), to-tickets (local tracker path `docs/tasks/<feature>/issues/`), triage (`.out-of-scope/` becomes `docs/out-of-scope/`), research (`docs/research/`), tdd (glossary line), improve-codebase-architecture (glossary and ADR paths; docs/specs/ as orientation input). Reference files inside skill folders are updated consistently with their SKILL.md, including GLOSSARY-FORMAT.md's variant paths (docs/glossary-map.md, `src/<context>/docs/glossary.md`). Each edited skill appends an entry to the import log.

Blocked by: 01

Status: resolved

- [x] None of the six edited skills references `.scratch/`, a root GLOSSARY.md, a root GLOSSARY-MAP.md, or `.out-of-scope/`.
- [x] Skills defer location truth to `docs/agents/` config where the config already carries it.
- [x] Reference files (ADR-FORMAT.md, GLOSSARY-FORMAT.md, OUT-OF-SCOPE.md) match their SKILL.md edits.
- [x] Import log has one entry per edited skill.

## Comments

Completed 2026-11-12, commits 0130fbd and 6e14e72 on main.

- domain-modeling: the file structure section defers to `docs/agents/domain.md` (defaults: `docs/glossary.md` and `docs/adr/`; multi-context variant: `docs/glossary-map.md` and `src/<context>/docs/glossary.md`), replacing the hardcoded root-file trees and their inference rules. GLOSSARY-FORMAT.md carries the same paths and deference; ADR-FORMAT.md defers multi-context ADR locations to `docs/agents/domain.md`.
- to-tickets: local tickets go under the tracker path the issue-tracker config defines, defaulting to `docs/tasks/<feature-slug>/issues/`, replacing `.scratch/<feature-slug>/issues/`.
- triage: the rejected-request KB moved to `docs/out-of-scope/` in SKILL.md, OUT-OF-SCOPE.md, and AGENT-BRIEF.md's example briefs; the glossary mention in the grill step is location-neutral (the location is domain-modeling's business).
- research: findings save to `docs/research/<topic>.md`, one file per topic and dated, replacing "wherever the repo keeps such notes".
- tdd: the glossary line reads the project's glossary at the location `docs/agents/domain.md` describes, `docs/glossary.md` by default.
- improve-codebase-architecture: glossary location follows `docs/agents/domain.md`, ADRs stay at `docs/adr/`, and a matching subsystem spec at `docs/specs/` is orientation input in the Explore step.
- Verification: case-insensitive grep over the six skill folders finds no `.scratch/`, root GLOSSARY.md/GLOSSARY-MAP.md, or `.out-of-scope/`; the remaining "glossary" mentions are location-neutral prose. Import log has six new README entries, one per edited skill. agents/openai.yaml files carry no location strings and were untouched.
- Code-review over b93a043...HEAD: standards axis found one hard violation (a unicode arrow left in the rewritten to-tickets bullet) fixed in 6e14e72; spec axis found the research dating clause and tdd's missing default fallback, both addressed in 6e14e72. Judged and kept: ADR-FORMAT.md's per-context ADR deference line (upstream's multi-context tree had per-context docs/adr/), and triage hardcoding `docs/out-of-scope/` (no docs/agents/ config carries the KB location, so the ticket's deference rule does not apply to it).
- Known gap for a later ticket: to-tickets and triage still tell the user to run `/setup-matt-pocock-skills`, a name this fork supersedes; that reference is not a moved location, so it stayed out of this ticket's mechanical scope. Ticket 04 builds `setup-engineering-skills`; the pointers need updating when it lands.
