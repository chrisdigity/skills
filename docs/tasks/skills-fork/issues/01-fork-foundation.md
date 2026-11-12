# 01: Fork foundation

What to build: Make this repository an installable skill source. Initialize git, copy every upstream skill verbatim into `skills/<name>/` (SKILL.md plus reference files), and scaffold the README import log with its first entries. Upstream's `setup-matt-pocock-skills` is not carried (superseded by setup-engineering-skills, ticket 04); `ask-matt` is carried verbatim here and renamed in ticket 02.

Blocked by: None (can start immediately)

Status: ready-for-agent

- [ ] Git initialized; initial commit contains skills/ and README.
- [ ] All 26 carried skills copied verbatim under `skills/<name>/`, each with its reference files (for example triage's OUT-OF-SCOPE.md and AGENT-BRIEF.md, domain-modeling's ADR-FORMAT.md and GLOSSARY-FORMAT.md, tdd's tests.md and mocking.md).
- [ ] README import log section records: one-directional derivation from upstream; skill names kept; `setup-matt-pocock-skills` superseded (replacement arrives in ticket 04).
- [ ] Layout verified against the CLI discovery rules (`skills/<name>/SKILL.md`, three-level walk). The slug-based smoke install runs after the human publish step (GitHub creation and first push).
- [ ] README notes the skills CLI prerequisite (Node.js 22.20 or later).
