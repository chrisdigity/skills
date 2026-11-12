# 05: Lifecycle edits

What to build: Behavior edits that make the governance model live in the flow. implement and implement-spec each gain a promotion close-out step after code-review: distill behavior into the matching subsystem spec at `docs/specs/<subsystem>.md`, hard-to-reverse decisions into `docs/adr/`, new terms into `docs/glossary.md`, then remove the promoted feature directory (its essence lives in the durable docs; git history retains the rest). code-review's spec-source search includes `docs/specs/`, and its Spec sub-agent brief gains the promotion-consistency check (a diff changing spec-described behavior must carry its promotion). to-spec gains the cross-feature dependency rule (dependencies on tracked-but-unpromoted features are surfaced in the feature spec and wired as ticket blocking edges) plus its path edits, since it is the same file.

Blocked by: 03

Status: ready-for-agent

- [ ] implement and implement-spec end with a promotion step whose completion criterion is checkable: specs updated, ADRs offered, glossary updated, tracker entries resolved, feature directory removed.
- [ ] code-review's spec-source search includes `docs/specs/`, and the Spec brief includes the promotion-consistency check.
- [ ] to-spec includes the cross-feature dependency rule.
- [ ] Import log entries for each edited skill.
