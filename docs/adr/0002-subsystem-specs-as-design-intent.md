# Subsystem specs are mutable design intent with a strict lifecycle

docs/specs/ holds one current-state document per subsystem, describing intended behavior as of the last promotion. Agents read specs as intent, code and tests as current truth, and docs/tasks/ as proposed work that may never land; conflicts between sources are surfaced, never silently resolved. Promotion is part of a feature's definition of done: implement and implement-spec distill completed features into specs, ADRs, and the glossary, and code-review checks that a diff changing spec-described behavior carries its promotion.

## Consequences

Specs drift when promotion is skipped; the code-review check is the enforcement, not convention. Specs carry no feature slugs, lifecycle lists, or proposed-work marks, so docs/specs/ reads as one coherent specification of current intent rather than a change log. Deferred and cancelled features leave no trace in specs: rejected requests live in docs/out-of-scope/ and decisions in docs/adr/.
