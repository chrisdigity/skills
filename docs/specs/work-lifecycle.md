# Work lifecycle

## Purpose

Work lifecycle is the subsystem that moves work from proposal to durable documentation. It covers where work is proposed and tracked, how it is sliced and reviewed, how completion is closed out by promotion, and which source answers which question when sources disagree.

## Behavior

Work is proposed as feature specs in the tracker. Before writing one, to-spec checks the tracker's open features for the subsystems the new work touches, so design accounts for tracked-but-unpromoted work that may change the same behavior. A feature spec surfaces each dependency on a tracked-but-unpromoted feature in a cross-feature dependencies section, naming the dependency, what this feature needs from it, and what happens if it does not land.

to-tickets slices a feature into tracer-bullet tickets, one file per slice with declared blocking edges; a ticket becomes unblocked exactly when every ticket it listed is resolved, and a blocker in another feature directory is referenced by that directory's tracker path.

implement and implement-spec build from the tickets or the spec, call tdd at pre-agreed seams where possible, and call code-review before the work lands. code-review runs two axes, standards and spec, as parallel sub-agents and reports them separately, so a change can fail one axis and pass the other without either masking the result.

Promotion closes out a completed feature and is part of its definition of done. Behavior distills into the matching subsystem spec, created when none covers the subsystem and named with its glossary term; hard-to-reverse decisions are offered as ADRs and written when the user confirms; new terms enter the glossary; tracker entries resolve; and the feature directory is removed last, once its essence lives in the durable docs. Promotion is done only when all five outcomes are checkable; git history retains whatever the durable docs do not.

code-review's spec axis enforces promotion consistency: a diff changing behavior a subsystem spec describes must carry its promotion, meaning the spec updated in the same change. Its spec-source search covers the tracker's feature spec and, for promoted changes whose feature directory is gone, the subsystem specs.

triage reads the rejected-request KB before evaluating a new request, so rejections deduplicate instead of re-litigating; an entry that passes the ADR bar graduates to the ADR directory and leaves the KB. research findings land as dated, one-file-per-topic records and are never promoted into a spec without re-verification.

Precedence decides which source answers which question: code and tests show what is, subsystem specs show what should be as of the last promotion, and the tracker shows what is proposed. Conflicts between sources are surfaced, never silently resolved. An agent checks the tracker's open features before designing against a subsystem spec.

## Interfaces and contracts

- Tracker shape: one feature per directory; the feature spec at that directory's root; one ticket file per slice under its issues directory, numbered from 01; a status line near the top of each ticket; conversation appended under a comments heading.
- The tracker's conventions and wayfinding operations (map, child tickets, blocked-by edges, frontier, claim, resolve) live in the managed issue-tracker configuration, which other skills read rather than restate.
- Subsystem spec shape: one file per subsystem, its slug the kebab-case form of the glossary term, carrying the four settled sections (Purpose, Behavior, Interfaces and contracts, Decisions in force) and none of the change-log marks: no feature slugs, no lifecycle lists, no proposed-work marks, no file paths.
- Rejected-request KB shape: one file per rejected concept, listing that concept's prior requests.
- Research shape: one file per topic, dated, cited.

## Decisions in force

- ADR 0002, subsystem specs are mutable design intent with a strict lifecycle: specs describe intended behavior as of the last promotion, the code-review check is the enforcement rather than convention, and rejected or cancelled work leaves no trace in specs.
