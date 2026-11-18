# Promotion and subsystem specs are removed; cleanup owns state removal

The promotion close-out and the subsystem-spec layer it fed are removed. A finished feature's state - its spec, tickets, comments, and research - is removed from the workspace by the cleanup skill once all its tickets resolve; the durable record is the glossary and the ADRs written inline as decisions land, and git history retains whatever the workspace drops.

Two observations forced the reversal. Lingering spec documents in the tracker invited misinterpretation: a session could read a completed feature's spec as guiding documentation for the whole project, which is the condition promotion was invented to prevent. And promotion's own distillation pass - behavior into subsystem specs, decisions into ADRs, terms into the glossary - duplicated what grill-with-docs and triage already write inline as decisions land. Paying for the duplication bought a subsystem-spec layer whose maintenance (promotion-consistency checks, spec-source searches, a precedence rule) outgrew its value.

The replacement keeps the original concern and drops the machinery. Tracker state is ephemeral: cleanup removes a feature's directory wholesale when its tickets resolve, verifies the containment invariant first (nothing outside docs/tasks/<name>/ references anything inside it), and protects the glossary, the ADRs, and the rejected-request KB - the documents that exist to persist and steer future agents. to-spec, implement, implement-spec, and code-review revert to upstream's shape; the fork's remaining differences are the recorded divergences.

Supersedes ADR 0002 (subsystem specs are mutable design intent with a strict lifecycle), which keeps its original wording as a dated record.

## Consequences

- The precedence rule (code and tests show what is, subsystem specs show what should be, the tracker shows what is proposed) loses its middle tier and is dropped entirely.
- Research is ephemeral and its life has three stages, each owned by an existing convention: working file (gitignored docs/tmp/<topic>.md, the disposable-docs convention), tracker state (docs/tasks/<feature>/research/, removed by cleanup with the feature), durable record (glossary and ADRs, distilled at write time). This supersedes the docs/research/ convention; ADR 0007's pointer to docs/research/combining-setup-and-update-agent-docs stands as written and resolves through git history.
- Research's file shape is uniform: always a research/ subdirectory with one file per topic, never a bare research.md - the directory is the linkage cleanup removes, and per-topic files keep parallel research subagents from sharing a write target.
- The containment invariant is enforced twice: by convention (durable documents never reference tracker state or research by path) and by verification (cleanup scans for inbound references before every removal and stops when one is found).
- cleanup is scoped to the local tracker by construction: on a hosted tracker the state lives on the host, so the local scan finds nothing to remove.
- ADRs 0003 through 0007 are unaffected: the managed-artifacts contract, the CLI delegation, the git policy, the skill names, and the setup/update merge stand.
