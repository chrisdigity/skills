# Fork adjustments: temp area, skill names, ledger, steering shape

## Problem

The fork's naming and layout accumulated drift across its first weeks. `.scratch/` served as both the upstream import's working directory and the de facto scratchpad while no document told agents where disposable files go. Two of the three fork-authored or renamed skills carried names that no longer state what they own. The divergence ledger's filename was longer than its content warrants. The steering file's research guidance was hand-added text the setup skill does not generate. The steering block's section order and Git heading were adjusted by hand with no generating template to match.

## Decisions

From the grilling session of 2026-11-16/17; each decision confirmed by the maintainer:

1. Shared temp area: `.scratch/` moves wholesale to gitignored `docs/tmp/`, flat layout: the upstream import's install (`docs/tmp/.agents/`, `docs/tmp/skills-lock.json`), the two baseline sets moved as-is, and agent throwaway files alongside.
2. A skill's name states what it owns (three-way swap): `setup-engineering-skills` -> `setup-agent-docs`, `update-engineering-skills` -> `update-agent-docs`, `import-upstream-skills` -> `update-engineering-skills`.
3. The divergence ledger becomes `docs/divergences.md`; H1 unchanged.
4. The import skill re-presents evolved divergences: a ledgered file whose upstream copy changed since the stamp's commit classifies as both sides instead of auto-defended. The stamp keeps replacing itself on every import.
5. The writing-standard block's management comment names the renamed skills; version stays v2 (wording unchanged, model-output effect none). The live block migrates through the maintainer's own update run.
6. The steering block template gains the current shape: Domain docs first, `### Git operations`, and new inline Research docs and Disposable docs sub-sections. No managed artifacts for those two: one-liner-sized content does not justify a per-repo file, and inline sub-section text is setup-written and update-frozen (accepted gap).
7. Setup owns the `.gitignore` entry for `docs/tmp/` (append when missing, create when absent); the update skill verifies its presence.
8. The git policy seed and artifact become `git-operations.md` titled "Git operations". Triage labels stay a separate heading and artifact (conditionally written when the triage skill is installed).
9. `LICENSE` becomes `LICENSE.md`.
10. No locale rule: carried text stays upstream's spelling verbatim; new fork-authored prose matches each file's existing spelling.
11. Inline one-liner pointer style: Research docs and Disposable docs carry no "See docs/agents/..." tail, since no artifact exists to point at.

## Out of scope

- ADRs 0001 through 0005 stay untouched as dated records, including their old skill names and paths.
- The `.scratch/` predecessor-detection sentence in setup's exploration step (historical detection, not placement guidance).
- Upstream-carried text mentioning `.scratch/`: baseline copies and the ledger's historical upstream-shape lines.
