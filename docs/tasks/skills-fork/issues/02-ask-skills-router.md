# 02: ask-skills router

What to build: Replace ask-matt with `skills/ask-skills/`: the same orientation role, rewritten for the new flow. The map covers the main flow (grill-with-docs, to-spec, to-tickets, implement and implement-spec with promotion close-out, retro), the on-ramps (triage, diagnosing-bugs), wayfinder, codebase health, the vocabulary layer, phase boundaries, and the standalone skills, with setup-engineering-skills and update-engineering-skills as the precondition pair. Uses the settled glossary vocabulary (tracker, feature spec, ticket, promotion, distillation, subsystem spec, precedence, managed artifact).

Blocked by: 01

Status: resolved

- [x] `skills/ask-skills/SKILL.md` exists with name and description frontmatter; no ask-matt directory remains.
- [x] The map references docs/tasks/, docs/specs/, docs/research/, docs/out-of-scope/, docs/glossary.md, setup-engineering-skills, and update-engineering-skills by name.
- [x] No reference to a root GLOSSARY.md or `.scratch/` remains in the router.
- [x] Import log entry records the rename and rewrite.

## Comments

Completed 2026-11-12, commits 5f1dadd and e281fcf on main.

- Router rewritten for the new flow: the precedence rule up front, promotion close-out (distillation into docs/specs/, docs/adr/, docs/glossary.md) in the main flow, triage reading the rejected-request KB, wayfinder as its own section (map at docs/tasks/<effort>/map.md), improve-codebase-architecture orienting on docs/glossary.md and docs/specs/, domain-modeling deferring to docs/agents/domain.md, and setup-engineering-skills / update-engineering-skills as the precondition pair. The pair's own skills arrive in tickets 04 and 06; the router points at the settled destination.
- PHASE-BOUNDARIES.md carried byte-identical into skills/ask-skills/; agents/openai.yaml carried with display_name "Ask Skills".
- Code-review run over 223304a...HEAD: spec axis clean against all four checklist items; standards axis findings addressed (decision-ticket disambiguation, update's raise-conflicts clause) or judged carried router genre (skill-name-led bullets, glossary-term summaries).
