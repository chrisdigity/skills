# 03: Location re-skin

What to build: Mechanical edits so no carried skill hardcodes a moved location: domain-modeling (file structure defers to docs/agents/domain.md), to-tickets (local tracker path `docs/tasks/<feature>/issues/`), triage (`.out-of-scope/` becomes `docs/out-of-scope/`), research (`docs/research/`), tdd (glossary line), improve-codebase-architecture (glossary and ADR paths; docs/specs/ as orientation input). Reference files inside skill folders are updated consistently with their SKILL.md, including GLOSSARY-FORMAT.md's variant paths (docs/glossary-map.md, `src/<context>/docs/glossary.md`). Each edited skill appends an entry to the import log.

Blocked by: 01

Status: ready-for-agent

- [ ] None of the six edited skills references `.scratch/`, a root GLOSSARY.md, a root GLOSSARY-MAP.md, or `.out-of-scope/`.
- [ ] Skills defer location truth to `docs/agents/` config where the config already carries it.
- [ ] Reference files (ADR-FORMAT.md, GLOSSARY-FORMAT.md, OUT-OF-SCOPE.md) match their SKILL.md edits.
- [ ] Import log has one entry per edited skill.
