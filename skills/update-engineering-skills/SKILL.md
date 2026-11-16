---
name: update-engineering-skills
description: "Pull upstream's changes into this fork: install mattpocock/skills side-by-side under docs/tmp/, sort every difference into upstream's new work, a divergence to defend, or drift to undo, and apply the confirmed import to skills/ and docs/divergences.md."
metadata:
  internal: true
disable-model-invocation: true
---

# Update engineering skills

Run this in the fork repository (chrisdigity/skills), where skills/ holds the sources. Two jobs, in order:

1. **Compare and classify**: install upstream side-by-side, diff, and sort every difference into the divergence ledger's three buckets.
2. **Apply the confirmed import**: carry the user's decisions into skills/ and the ledger.

The ledger at docs/divergences.md is the contract: every live divergence is named there with its reason. This skill never introduces an undocumented divergence: a difference ends the run adopted, defended by a ledger entry, or undone. In a consuming repo (no skills/ directory at the root) there is nothing to import into: say so and stop.

This is the fork-side counterpart to update-agent-docs, which refreshes consuming repos from this fork after the user commits and pushes.

## Process

### 1. Install upstream side-by-side

docs/tmp/ is gitignored working state, and a stale comparison is worse than none, so start clean:

- Delete docs/tmp/.agents/ and docs/tmp/skills-lock.json when present.
- From inside docs/tmp/: `npx skills add mattpocock/skills -y`. The -y skips the scope prompt, installing into docs/tmp/.agents/skills/.
- The CLI errors when Node is older than 22.20 or the network is unreachable: report the error and stop.

### 2. Pair and diff

Pair every folder in skills/ with its upstream counterpart in docs/tmp/.agents/skills/. The rename map:

- ask-skills pairs with upstream's ask-matt
- setup-agent-docs pairs with upstream's setup-matt-pocock-skills

(When the ledger records a new rename, add its pair here.)

Sort the unpaired folders:

- Fork-only (currently update-agent-docs and update-engineering-skills): fork-authored, no diff.
- Upstream-only: a new upstream skill, an import candidate the user decides on.
- A carried skill missing upstream entirely: upstream removed it; the user decides whether the fork maintains it from here on (record that in the ledger) or drops it.

Per paired folder, run `git diff --no-index --stat skills/<name> docs/tmp/.agents/skills/<upstream-name>`, then read the full diff per differing file. When a diff shows every line changed, check line endings before reading content: CR/LF noise masks real edits.

### 3. Attribute each difference

The fork's history is the attribution instrument. The foundation commit 43dd672 ("Fork foundation: carry 26 upstream skills verbatim") holds upstream-at-import content for the carried skills, under upstream's names for the renames. For each differing file, take the first classification that fits:

1. **Ledgered**: a live entry in docs/divergences.md explains the difference. When upstream's copy of the file matches the copy at the stamp's commit (one raw fetch, the same mechanism as the fallback baseline below), the difference is exactly the ledgered divergence: defend, no action, list it as defended. When upstream's copy has changed since the stamp, upstream did new work in a file the fork defends, so classify as both sides (4) instead: the entry and upstream's evolution are presented side by side, and the user decides whether the entry still defends.
2. **Fork-side, undocumented**: `git log --oneline -- skills/<name>/<file>` shows fork commits after the baseline touching the file, and no ledger entry explains the change. Drift candidate: the user restores upstream's version (undo) or keeps it and the import adds a ledger entry (defend).
3. **Upstream-side**: `git show <baseline>:skills/<upstream-name>/<file>` matches the fork's current copy, so the fork never touched the file and the whole difference is upstream's new work since the recorded import. Import candidate; note when the file also carries ledgered divergences, because adopting means merging them.
4. **Both sides**: fork commits touch the file and the baseline differs from upstream-now. Present the fork's divergence and upstream's change side by side; the user decides the merge.

The baseline is upstream-at-import for the file, looked up in order:

1. Fork history: `git show 43dd672:skills/<upstream-name>/<file>`. The setup skill was authored fresh, never carried verbatim, so its baseline is not in history.
2. Raw fetch of the recorded upstream commit: `https://raw.githubusercontent.com/mattpocock/skills/<upstream-commit>/<source-path>`, where the ledger's header gives <upstream-commit> and docs/tmp/skills-lock.json gives <source-path> (its skillPath entries name upstream's category-nested paths).

After a merged import, fork history no longer holds a pristine upstream snapshot, and the raw fetch becomes the primary baseline. That is expected: the ledger's stamp exists for exactly this.

### 4. Present the plan and confirm

Present the plan before any write, one line per action, grouped by bucket:

- **Import**: adopt upstream's change (file, what changed, merge notes when the file carries divergences).
- **Defend**: keep the fork's version; the ledger entry exists, or the drafted entry is shown.
- **Undo**: restore upstream's version, dropping an undocumented fork edit.
- **Adopt**: carry a new upstream skill into skills/ (folder flattened out of its category, name kept).
- **Skip**: no action this import.

Wait for an explicit yes. No write happens before it.

### 5. Apply

- Import: apply upstream's change to the fork's copy. When the file carries divergences, hand-merge: upstream's change in, ledgered divergences intact.
- Adopt: copy the folder from docs/tmp/.agents/skills/<name> to skills/<name>.
- Undo: copy the file from docs/tmp/.agents/skills/<upstream-name>/.
- Ledger: add entries for newly defended divergences, remove entries for retired ones, and update the header stamp with the upstream commit and date this import carried. One API call fetches the hash (`https://api.github.com/repos/mattpocock/skills/commits?per_page=1`); when offline, keep the previous stamp and say so.
- README: update the skill counts when a skill was adopted.
- Stage nothing, commit nothing: the user reviews the diff and commits, and agents never push on their own (docs/agents/git-operations.md).

### 6. Done

Report what was imported, defended, undone, and adopted, and the ledger's new stamp. Done when all three are checkable:

- Re-running the paired-folder diffs leaves only differences a ledger entry documents.
- The ledger's stamp names this import's upstream commit and date.
- git status shows changes only under skills/, docs/divergences.md, and README.md.

Then note the handoff: consuming repos pick the import up with `npx skills update` followed by /update-agent-docs; in this repo, refresh the test install with `npx skills add chrisdigity/skills` so .agents/skills mirrors sources.
