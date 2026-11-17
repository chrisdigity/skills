# Storing update-engineering-skills as a committed project skill

Question: can update-engineering-skills leave the distributed skillset (`skills/`) and live as a committed project skill at `.agents/skills/update-engineering-skills/`, kept under version control through a gitignore exception?

Decision (2026-11-17): the maintainer kept the skill in `skills/` after all. Two findings from this investigation were adopted regardless: the CLI's `INSTALL_INTERNAL_SKILLS=1` environment variable (confirmed against the CLI's code and docs, and verified end to end) is the official way to install and view internal skills, recorded in `skills/update-engineering-skills/notes.md`; and the stale installed copy at `.agents/skills/update-engineering-skills/` (a pre-rename snapshot of the retired migration skill, lock entry pinned to commit 785a755) was replaced with the current tool.

Investigated 2026-11-17. Findings are empirical where marked, verified against the skills CLI's actual behavior in `docs/tmp/` scratch installs.

## Verdict

Yes, the placement is mechanically sound and appropriate. The CLI tolerates untracked folders inside `.agents/skills/`, the gitignore exception works with the parent-chain pattern, and OpenCode discovers the folder as a skill. Two facts sharpen the rationale: `metadata.internal: true` already keeps the skill out of every consumer install, so the move changes nothing about distribution - it makes the source tree's `skills/` exactly the shipped set, which is the real gain. And the move overwrites a stale zombie the test install currently carries at that exact path.

## Empirical findings

1. **Non-distribution is already in force.** The published source (28 skills after the merge) reports "Found 27 skills" to a consumer install and skips `update-engineering-skills` entirely; `npx skills check` does not even scan it as part of the source's visible set. Verified by installing the published fork into `docs/tmp/.agents/` and by the root check's flagged-deleted list. The `metadata.internal: true` frontmatter flag is what does this.
2. **Untracked folders in the install directory are tolerated.** A folder in `.agents/skills/` with no lock entry is listed by `npx skills list` as `Source: untracked`, discovered for OpenCode, and survives re-runs of `npx skills add` and `npx skills update` without deletion or warning. `npx skills check` ignores it. Verified with a scratch skill in `docs/tmp/.agents/skills/`.
3. **The gitignore exception needs the parent chain.** The current rule `.agents/` excludes the directory itself, and git cannot re-include a child of an excluded directory. The working rule set (verified in a scratch repo, mirror stays ignored):
   ```
   .agents/*
   !.agents/skills/
   .agents/skills/*
   !.agents/skills/update-engineering-skills/
   ```
4. **The test install carries a zombie at the destination path.** `.agents/skills/update-engineering-skills/` holds a pre-rename snapshot of the retired migration skill (it still references `setup-engineering-skills`, `git.md`, and writing-standard v1), with a lock entry pinned to commit 785a755. The move overwrites it, and its lock entry is removed, so the CLI classifies the folder as untracked and never touches it again.

## The trade-off

`skills/` becomes purely the distributed set (27 carried skills), the import's fork-only sorting category becomes empty, and the tool's provenance moves from "fork-authored skill in the shipped set" to "repository maintenance tooling, version-controlled but not distributed". The cost is a mixed-semantics directory: `.agents/skills/` is otherwise 100% derived state, so the one committed folder is an exception future maintainers must have documented. Mitigation: say so in the README, the divergences ledger, and the skill's own header.

## Changes the move requires

- Move `skills/update-engineering-skills/` to `.agents/skills/update-engineering-skills/`, overwriting the zombie; drop the now-moot `metadata.internal` flag; add a header line stating where the skill lives and that it is not distributed.
- Replace the `.agents/` gitignore rule with the parent-chain rule set above.
- Remove the skill's stale root lock entry (commit 785a755) so the CLI sees an untracked folder, not a deleted-upstream skill.
- README: 27 carried skills, no fork-authored skill in the set; the maintenance tool noted separately.
- Divergences ledger: reclassify the entry from fork-authored-shipped-skill to repo-maintenance tooling.
- Its own pairing instruction "Fork-only (currently update-engineering-skills)" becomes vacuous and is reworded.
- `docs/specs/skills-distribution.md`: one line recording that repository-maintenance tooling lives outside the distributed set.
