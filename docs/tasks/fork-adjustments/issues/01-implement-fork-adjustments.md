# Implement fork adjustments

Status: ready-for-agent

Implements `docs/tasks/fork-adjustments/spec.md` in one pass, ending with an uncommitted diff for maintainer review. No staging, no committing, no pushing (git policy).

## Checklist

- [x] Move `.scratch/` to `docs/tmp/` (flat layout) and delete `.scratch/`
- [x] Rename skills: setup-engineering-skills -> setup-agent-docs; update-engineering-skills -> update-agent-docs; import-upstream-skills -> update-engineering-skills (folders, frontmatter, H1s, descriptions, cross-references)
- [x] Update the carried `/setup-agent-docs` pointer sites (to-tickets x2, to-spec, implement-spec, code-review, triage, wayfinder) and ask-skills' router entries
- [x] Rename the ledger to `docs/divergences.md`; move every live reference; add the docs/tmp and LICENSE.md shape lines
- [x] Add the re-presentation rule to the import skill's classification 1
- [x] Update the writing-standard seed's management comment (names only, version stays v2)
- [x] Reshape the setup block template (order, Git operations heading, Research docs, Disposable docs) and the Section D references
- [x] Add the .gitignore ensure step to setup and the presence check to update
- [x] Rename the git seed and this repo's artifact to `git-operations.md`, titled "Git operations"
- [x] Replace this repo's AGENTS.md research section with the two inline sub-sections; update the git pointer
- [x] `LICENSE` -> `LICENSE.md`; README links and counts
- [x] Bring the glossary and both subsystem specs current; draft ADR 0006
- [x] Verification: grep sweep for stale names and paths; discovery reports 28 skills (29 with internals)

## Comments

Implemented 2026-11-17. The diff awaits maintainer review; renames are staged (inherent to git mv), all other edits unstaged, nothing committed or pushed. The live writing-standard block in AGENTS.md still carries the old skill names by design: the maintainer's update-agent-docs run migrates it from the seed.
