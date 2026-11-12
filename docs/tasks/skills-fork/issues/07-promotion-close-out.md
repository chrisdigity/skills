# 07: Promotion close-out of skills-fork

What to build: Exercise the promotion step this fork defines, on this feature. Distill the completed work into `docs/specs/` subsystem specs (expected subsystems: skills distribution, project configuration, work lifecycle), promote any newly crystallized terms into `docs/glossary.md`, resolve the tracker entries, and remove the promoted feature directory per the distillation rule. Run code-review over the tickets' integration so the promotion-consistency check is exercised on real diffs (seam C).

Blocked by: 02, 04, 05, 06

Status: ready-for-agent

- [ ] `docs/specs/` contains one spec per subsystem, each with the four settled sections (Purpose, Behavior, Interfaces and contracts, Decisions in force).
- [ ] No feature slugs, lifecycle lists, or proposed-work marks anywhere in `docs/specs/`.
- [ ] Glossary updated with any terms that crystallized during the build.
- [ ] Feature directory removed after promotion; durable essence present in docs/specs/, docs/adr/, and docs/glossary.md.
- [ ] Code-review run recorded, with the promotion check exercised.
