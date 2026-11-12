# 01: Fork foundation

What to build: Make this repository an installable skill source. Initialize git, copy every upstream skill verbatim into `skills/<name>/` (SKILL.md plus reference files), and scaffold the README import log with its first entries. Upstream's `setup-matt-pocock-skills` is not carried (superseded by setup-engineering-skills, ticket 04); `ask-matt` is carried verbatim here and renamed in ticket 02.

Blocked by: None (can start immediately)

Status: resolved

- [x] Git initialized; initial commit contains skills/ and README.
- [x] All 26 carried skills copied verbatim under `skills/<name>/`, each with its reference files (for example triage's OUT-OF-SCOPE.md and AGENT-BRIEF.md, domain-modeling's ADR-FORMAT.md and GLOSSARY-FORMAT.md, tdd's tests.md and mocking.md).
- [x] README import log section records: one-directional derivation from upstream; skill names kept; `setup-matt-pocock-skills` superseded (replacement arrives in ticket 04).
- [x] Layout verified against the CLI discovery rules (`skills/<name>/SKILL.md`, three-level walk). The slug-based smoke install runs after the human publish step (GitHub creation and first push).
- [x] README notes the skills CLI prerequisite (Node.js 22.20 or later).

## Comments

Completed 2026-11-12, root commit 43dd672 on main.

- 26 skills copied verbatim from upstream (19 engineering, 7 productivity), byte-identical per `diff -r` against upstream at commit 4ae2317. `setup-matt-pocock-skills` excluded; `ask-matt` carried verbatim.
- README import log records the one-directional derivation policy, kept skill names, the layout flattening (`skills/<category>/<name>/` to `skills/<name>/`), and the setup supersession. README notes the Node.js 22.20+ prerequisite.
- Layout verified against the skills CLI discovery docs (vercel-labs/skills, creating-skills.md): all 26 skills sit at `skills/<name>/SKILL.md` with `name` and `description` frontmatter, one SKILL.md per skill, inside the three-level walk. The slug-based smoke install stays with the human publish step.
- LICENSE carried unchanged (MIT, Matt Pocock); the carried skills are upstream's copyrighted work, so the notice travels with them.

