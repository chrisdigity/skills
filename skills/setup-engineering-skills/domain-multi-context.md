# Domain docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase. This repo uses the multi-context layout: one glossary per context, with a map pointing at them.

## Before exploring, read these

- **`docs/glossary-map.md`**: points at one glossary per context; read each glossary relevant to the topic
- **`docs/specs/`**: the current-state spec for any subsystem you're about to work in
- **`docs/adr/`**: read ADRs that touch the area you're about to work in; also check `src/<context>/docs/adr/` for context-scoped decisions

If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill (reached via `/grill-with-docs` and `/improve-codebase-architecture`) creates the glossaries and ADRs lazily, when terms or decisions actually get resolved; a subsystem spec appears at its subsystem's first promotion.

## Precedence

When sources disagree about the same behavior: code and tests show what **is**; a subsystem spec at `docs/specs/` shows what **should be**, as of its last promotion; the tracker at `docs/tasks/` shows what is **proposed**, which may never land. Surface a conflict between sources rather than resolving it silently.

Before designing against a subsystem spec, check the tracker's open features for that subsystem: proposed work there may change the spec's answers.

## File structure

Multi-context layout:

```
/
|-- docs/
|   |-- agents/          this config
|   |-- adr/             system-wide decisions
|   |-- glossary-map.md  points at one glossary per context
|   `-- specs/           current-state subsystem specs
`-- src/
    |-- ordering/
    |   `-- docs/
    |       |-- adr/     context-scoped decisions
    |       `-- glossary.md
    `-- billing/
        `-- docs/
            `-- glossary.md
```

## Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in the relevant context's glossary. Don't drift to synonyms the glossary explicitly avoids.

If the concept you need isn't in the glossary yet, that's a signal: either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts ADR-0007 (event-sourced orders), but worth reopening because..._
