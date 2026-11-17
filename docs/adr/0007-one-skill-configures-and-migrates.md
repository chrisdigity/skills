# One skill configures and migrates the managed artifacts

setup-agent-docs and update-agent-docs were a pair: one wrote the managed artifacts through a question flow, the other migrated them through classification against the seeds, and each carried half of a loop the other lacked. The split made reconfiguration and maintenance different commands with different guarantees, and a maintainer had to know which to run. The merged skill is one install/reinstall pass: every run prepares all of the section questions setup asked on a first run and asks them together in one message, with the currently recorded configuration as the recommended answers, and the migration checks (seed drift as diffs, conflicts, unattributable files, newly managed artifacts) are presented in the same pass's plan. Prior output changes the answers' defaults, not the flow: the loop is presence-driven, not mode-driven (docs/research/combining-setup-and-update-agent-docs).

Three refinements were decided with the merge:

1. The skill runs no skills-CLI step. Installing or updating skills is entirely the caller's CLI job; ADR 0004's delegation stands, and the merged skill goes further than its predecessor by running no CLI command at all, so it never writes outside the managed artifacts. Seeds are read from the skill's own folder as installed, which dissolves the pair-ships-together dependency: migration against the installed seeds is version-coherent with the installed skills, because seeds and skills ship in one install.
2. Recorded answers are reconfigurable by answering the section's question. Nothing sits behind an immutable "user-owned" wall: the push answer, the freeform workflow text, and the tracker variant all change by answering their section differently, and the files are rewritten from the user's words. What remains true is that migration never rewrites them silently.
3. ADR 0005's "once adopted, the sub-block's presence ends the asking" is superseded: the git section re-asks on every run with the recorded answer as the default, and a declined policy is re-offered, because a decline leaves no record. The acceptance gate (never ship uninvited) and the never-silently-rewritten rule stand.

update-agent-docs is deleted; its migration process is absorbed into setup-agent-docs. The writing-standard block's management comment names one skill (version stays v2, per the ADR 0006 precedent).

## Consequences

Consuming repos run one command for both configuration and maintenance; the after-import handoff becomes `npx skills update` followed by `/setup-agent-docs`. The merged SKILL.md is longer and carries per-artifact conditionals, and the paired upstream skill (setup-matt-pocock-skills) now diverges by a whole process rather than a restructure, so imports attribute a larger fork-side surface in that file: the accepted cost of the single command. ADRs 0003 through 0005 keep their original wording as dated records; this ADR and the spec carry the current shape.
