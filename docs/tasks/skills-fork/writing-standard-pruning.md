# Writing-standard pruning proposal

A change list for the writing-standard block, analyzed with the writing-for-agents levers. Nothing here is applied: this repository's steering-file block and the v1 seed in the setup skill's folder carry identical text, both are untouched by this ticket, and the gate stays shut until the maintainer approves the list (ADR 0001). On approval the changes ship as v2 of the block: the seed is edited first, and update-engineering-skills replaces the steering-file block in place by its delimiters.

Changes are limited to restatement and overlap, except change 3, which carries a second, separately justified fix inside text it already relocates.

## Change 1: cut the puffery demonstration (restatement)

Before:

```
#### Bad (Significance Puffery)
> The primary database serves as the central nexus of the architecture, fostering seamless data flow across services while marking a pivotal milestone in system reliability.
```

After: deleted.

Reasoning: every phrase the demonstration exhibits is already enumerated in the prose. "serves as" is in rule 2's cut-list; "fostering" and "seamless" fall under rule 3's word list; "pivotal" and "marking a ... milestone" are in rule 1's cut-list ("pivotal", "marks a shift"). Under the relevance lever, a line earns its always-loaded cost only by bearing on what the document does; a demonstration whose content is fully enumerated one heading up is restatement, paying context load every turn without changing what the model matches against. Counter-risk, stated for the gate: demonstrations steer output more strongly than enumerations for some models, so the cut trades demonstration pull for context economy.

Expected effect on model output: matching continues against rules 1-3's cut-lists, which name every phrase the demonstration carried; the block is four lines shorter in every session; no rule is weakened because no rule is touched.

## Change 2: merge the two technical-explanation bullets (overlap)

Before:

```
- **Preserve causal glue:** When stating a design choice or constraint, explicitly state the underlying physical or architectural mechanism (*"System X requires component Y because process Z overwrites local state"*).
- **Retain concrete data:** Never drop file paths, numbers, error codes, configuration keys, or explicit dependencies for the sake of brevity.
```

After:

```
- **Preserve the mechanism:** When stating a design choice or constraint, explicitly state the underlying physical or architectural mechanism (*"System X requires component Y because process Z overwrites local state"*), and keep the concrete data that carries it: file paths, numbers, error codes, configuration keys, explicit dependencies.
```

Reasoning: both bullets enforce one invariant, that compression must not destroy the mechanism, split into a why-half and a what-half. At application time the boundary is unanswerable: a version number, a file path inside a causal claim, an error code in an explanation, each is both "causal glue" and "concrete data", so the model spends a boundary guess the split creates and no behavior change follows from answering it. Under the duplication lever (one meaning in one place), the merged bullet is the single statement; both halves' enumerated content survives verbatim.

Expected effect on model output: one hook to match against instead of two; the data cut-list is applied as part of the same check as the mechanism statement; the section drops from two rules to one with no content removed.

## Change 3: dissolve the Examples section (restatement, co-location) with a correctness fix in transit

Before:

```
#### Bad (Over-Compressed Shorthand)
> Storage layer: unchanged — local engine, zero external deps.

#### Good (Plain Technical Explanation with Causal Glue)
> The storage layer remains unchanged because the engine executes locally on disk, requiring no external network services or third-party credentials.
```

After: the Examples section is deleted; the pair moves inline under the merged bullet from change 2:

```
- **Preserve the mechanism:** When stating a design choice or constraint, explicitly state the underlying physical or architectural mechanism (*"System X requires component Y because process Z overwrites local state"*), and keep the concrete data that carries it: file paths, numbers, error codes, configuration keys, explicit dependencies.
  Bad: "Storage layer: unchanged, local engine, zero external deps." Good: "The storage layer remains unchanged because the engine executes locally on disk, requiring no external network services or third-party credentials."
```

Justification a (restatement, co-location): the "Over-Compressed Shorthand" label names a rule the prose never states; the pair actually demonstrates the technical-explanations section, the Good example's "because" clause being the mechanism rule and its concrete detail the data half. Co-location keeps a concept's rule and its demonstration under one heading; today they sit three headings apart under an Examples heading that carries no rule of its own. With changes 1 and 3 the section is empty and dissolves.

Justification b (separate, correctness): the quoted bad example contains an em dash, which the block's own plain-ASCII rule bans. Per the negation lever, a forbidden token in always-loaded context models the violation in every session that reads the block. The comma keeps the telegraphic rhythm that makes the example bad; the dash is not the compression signature being demonstrated.

Expected effect on model output: the demonstration is matched at the rule it belongs to; the orphaned label disappears; the block stops carrying an em dash; the section count drops by one.

## Considered and not proposed

- Rule 6 (fluff trailing analysis) against rule 1 (significance puffery): distinct failure modes, a trailing clause versus a whole sentence, so neither subsumes the other.
- The plain-ASCII bullet against the file-trees bullet: a ban plus a positive instruction, complementary rather than duplicated.
- The core rule's closing sentence ("Fix what the sentence is doing, not just isolated words"): load-bearing scope for all eight rules, not restatement.
