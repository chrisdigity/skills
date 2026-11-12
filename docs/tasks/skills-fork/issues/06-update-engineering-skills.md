# 06: update-engineering-skills

What to build: The new update skill. It delegates skill-file refresh to the skills CLI (`npx skills check`, then `npx skills update`) and spends its own instructions on migration: inventory the managed artifacts (`docs/agents/*.md` and the writing-standard block), present a migration plan to the latest model, confirm before any write, regenerate managed artifacts, preserve unmanaged content under `docs/` untouched, and raise conflicts (for example a custom file shadowing a new managed artifact) rather than overwrite.

Blocked by: 04

Status: ready-for-agent

- [ ] `skills/update-engineering-skills/SKILL.md` exists.
- [ ] In the scratch repo from ticket 04: a hand-edited managed artifact regenerates after confirmation; a custom `docs/` subdirectory survives untouched; the conflict case raises instead of overwriting (seam B).
- [ ] The skill does not implement its own installer; file refresh goes through the CLI.
- [ ] Import log entry.
