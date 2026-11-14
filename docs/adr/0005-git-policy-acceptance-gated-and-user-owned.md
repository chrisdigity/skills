# The git policy is acceptance-gated and user-owned

ADR-0003 has update regenerate managed artifacts byte-identical from their seeds, which would reset the git policy's recorded push answer and any maintainer-recorded commit requirements on every migration, and would ship the artifact into repos that declined it. Setup therefore writes `docs/agents/git.md` only when the user accepts it, the steering block's `### Git` sub-section records that acceptance, and update manages the file only while that sub-section is present. The file is never regenerated: seed changes are presented as a diff and adopted only by explicit choice, on the same terms as the freeform tracker.

## Consequences

Generalized guidance in the seed, such as the failure protocol, reaches adopted repos only through the asked prompt, not automatically. A decline leaves no record, so setup re-asks on its next run; the `### Git` sub-section's presence is what ends the asking. A sub-section removed by hand while the file remains is raised once as unattributable and left alone unless the user says otherwise.
