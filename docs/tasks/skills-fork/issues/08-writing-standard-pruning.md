# 08: Writing-standard pruning proposal

What to build: The gated review artifact: a change list proposing exact wording changes to the writing standard, analyzed with the writing-for-agents levers. The likely targets are restatement (the examples section re-demonstrates rules the prose already states; the technical-explanations section overlaps the retain-concrete-data rule), but no rule is removed without explicit justification. Each proposed change carries its exact before and after text, the reasoning, and the expected effect on model output. No change is applied anywhere; the gate stays shut until the change list is approved.

Blocked by: 04

Status: resolved

- [x] Proposal saved at `docs/tasks/skills-fork/writing-standard-pruning.md`.
- [x] Every proposed change shows exact before/after text, reasoning, and expected effect on model output.
- [x] Changes are limited to restatement and overlap unless a separate justification is given for each.
- [x] The writing standard itself is unmodified by this ticket.

## Comments

Completed 2026-11-13.

- Three changes proposed: the puffery demonstration cut (every phrase it exhibits is already in the rules 1-3 cut-lists); the two technical-explanations bullets merged into one mechanism rule (both stated one invariant, and the boundary between causal glue and concrete data was unanswerable at application time); the Examples section dissolved (the compression pair moves inline under the rule it demonstrates, with the em dash in the quoted bad example fixed in transit under its own justification).
- Analysis used the writing-for-agents levers: relevance and pruning for the cut, duplication and single-source-of-truth for the merge, co-location for the relocation, negation for the dash fix. Considered and not proposed, with the distinctness judgment recorded in the proposal: rule 6 vs rule 1, the ASCII bullet vs the file-trees bullet, the core rule's closing sentence.
- The gate is shut: no wording changed anywhere. The steering-file block and the v1 seed are byte-identical to before this ticket. On approval the list ships as v2 through update's in-place block replacement, seed first.
- This proposal dies with the feature directory at ticket 07's promotion (distillation rule); after promotion, approval resumes from git history at this path.
