# Combining setup-agent-docs and update-agent-docs

Question: can one skill replace the setup/update pair, where a re-run installs what is missing and offers seed diffs for what drifted, without the skill operating under two modes?

Investigated 2026-11-17. Sources: `skills/setup-agent-docs/SKILL.md` and `skills/update-agent-docs/SKILL.md` (source and installed copies verified identical), `docs/specs/project-configuration.md`, ADRs 0001, 0003, 0004, 0005, and 0006, `docs/divergences.md`, `docs/glossary.md`, and the cross-references in `ask-skills`, `update-engineering-skills`, and the six skills that point missing configuration at `/setup-agent-docs` (to-spec, code-review, implement-spec, to-tickets, triage, wayfinder).

## Verdict

The merged workflow does not need two modes. The behavior described for the combined skill is one loop over managed artifacts where each artifact's presence selects the interaction:

- Artifact absent -> create it, asking the questions that artifact needs (setup's Sections A through D become per-artifact questions).
- Artifact present -> classify it against the latest seed and offer the diff or preserve it (update's steps 3 through 5).

The "mode" is emergent from repo state, not a switch. Both halves of this loop already exist in fragments across the pair:

- setup already re-runs state-dependently: step 1 says a re-run "updates the existing files in place rather than re-asking settled questions," and ADR 0005 has setup re-ask the git policy only while the acceptance record (`### Git operations`) is absent.
- update already handles absence and creation: missing artifacts are offered from the latest seed, conflicts are presented side by side for adoption or kept custom.

An earlier conclusion that combining "required the setup skill to operate under 2 different modes" reads as a conflation of state-dependent behavior with a mode switch. State-dependent behavior is inherent to the combined design and is already present in both skills.

## What each skill does that the other does not

setup only:

- Configuration questions: tracker variant (Section A), triage label overrides (B), domain variant (C), git-policy acceptance and push answer (D).
- Constructing the steering file's `## Agent skills` block from scratch, choosing `CLAUDE.md` vs `AGENTS.md`, creating either when neither exists.
- The `.gitignore` `docs/tmp/` entry; `gh`/`glab` label creation for hosted trackers; monorepo detection; the `.scratch/` migration offer.

update only:

- Skills-CLI refresh (`npx skills check`, then `update`, flagging local changes) per ADR 0004; setup implements no installer and runs no CLI step.
- Classification of every artifact against the latest seed (match / same shape / different shape / freeform / user-owned / missing / unattributable), plan-then-confirm, byte-identical regeneration, conflict raising, in-place replacement of the writing-standard block.
- The never-switch rule: migration never changes the tracker or domain variant and never re-asks settled questions.

## Where a real line remains: intent, not state

One distinction survives consolidation and is worth keeping explicit. Migration is mechanical: present artifacts are compared to seeds, never reconfigured. Reconfiguration (switch tracker, restart from scratch) is an explicit user request with destructive potential, and setup's step 5 already scopes re-runs to exactly those cases. In one loop this is a per-artifact branch the user drives in the moment ("this file is current; say the word if you want it to configure something else"), not a top-level mode. The consolidated skill keeps update's never-switch rule for its migration arm.

## What a full merge costs

1. Upstream diffability. `setup-agent-docs` pairs with upstream's `setup-matt-pocock-skills` through the rename map (`update-engineering-skills` line 35), and `update-agent-docs` is fork-authored with no upstream counterpart (divergences line 33), so its diffs are never attributed during imports. Folding the migration process into the paired file puts fork-only text where every import must re-attribute it. The seeds are already shared (update reads setup's folder), so sharing is not the issue; the SKILL.md process text is.
2. Prompt length and conditionals. The merged SKILL.md roughly doubles and carries per-artifact conditionals (ask-to-create vs diff-vs-seed vs conflict vs the freeform and user-owned carve-outs). Prompt-driven skills drift more as conditional depth grows; the current split keeps each run's instruction surface at one purpose.
3. One-time documentation sync, mechanical but wide: `ask-skills` router entries (lines 105-106), `update-engineering-skills` pairing and handoff text (lines 18, 35, 41, 92), setup step 5 and update step 6 cross-references, the seed cross-references (`issue-tracker-freeform.md` line 5, the `writing-standard.md` delimiter comment), `docs/specs/project-configuration.md` (two behavior paragraphs become one), glossary entries, a new ADR, a divergences entry, and the README skill count (29 -> 28).

## What a full merge gains

1. One command: re-running setup installs what is missing and migrates what drifted.
2. The pair-ships-together dependency dissolves: seeds live in the merged skill's own folder, removing the ADR 0004 cross-skill read and update's missing-sibling stop.
3. The CLI refresh runs on every invocation, so a forgotten migration self-heals on the next setup run.
4. update's "setup has never run, point at /setup-agent-docs" exit (step 3) disappears; the six missing-config pointers in other skills keep working unchanged.

## The shared-procedure alternative

The convenience goal does not require a full merge. Extract update's migration procedure (steps 2 through 6, minus the CLI step) into a reference file in setup's folder next to the seeds, and have both skills follow it:

- setup's step 1 gains a branch: when prior output exists (`docs/agents/` or a delimited writing-standard block), run the CLI check/update, then execute the shared migration procedure, then return to the section questions only for artifacts the user explicitly asks to reconfigure.
- update becomes a thin entry point: CLI refresh, then the same shared procedure. It stays for the after-import handoff (`update-engineering-skills` line 92) and can be retired later if the single-command flow sticks.

This reaches the stated convenience with a one-time restructure: no duplicated process text, no import-noise increase (setup gains one line; the reference file is fork-only and trivially recorded in divergences), and each entry point stays short. It also does not foreclose the full merge later.

## Behavior changes a consolidation must record

1. The git policy's acceptance gate. update offers `git-operations.md` only when the `### Git operations` sub-block is present and otherwise "leaves it to setup." In a consolidated loop that distinction collapses into the absent-artifact question: want the git policy reference? ADR 0005's re-ask rule is retained unchanged by maintainer decision: a decline leaves no record, so the question fires again on the next run, and the sub-block's presence is what ends the asking.
2. The writing-standard delimiter comment names the pair ("setup-agent-docs writes this; update-agent-docs replaces it in place"). Consolidation changes that comment; the version stays v2, matching the ADR 0006 precedent where the comment changed without a version bump.

## Decision

The blocker to combining was never mechanical; the user's single-loop reading is correct. The maintainer chose the full merge on 2026-11-17, rejecting the shared-procedure alternative as an unnecessary detour, and required that ADR 0005's re-ask rule be retained. ADR 0007 records the consolidation. Three refinements were decided during design:

1. No skills-CLI step: the skill never runs `npx skills check`/`update` and never writes outside the managed artifacts. Skill-file refresh is the caller's CLI job, and the seeds are read from the skill's own folder as installed, so a migration is version-coherent with the installed skills. This drops the earlier conclusion that the merged flow should refresh skill files itself.
2. Recorded answers are reconfigurable by answering the section's question; no "user-owned" wall makes any part of the setup immutable. The push answer, the freeform workflow text, and the tracker variant all change through their sections, rewritten from the user's words; migration still never rewrites them silently.
3. A re-run is a reinstall walk: every run re-asks the same section questions first-run setup asks, with the recorded configuration as the recommended answer. Nothing about the first-run functionality is lost on a re-run.
