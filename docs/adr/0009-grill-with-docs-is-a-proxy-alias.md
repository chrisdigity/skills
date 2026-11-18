# grill-with-docs is a proxy alias, not an invocable module

grill-with-docs carries no implementation of its own: it is a human-facing alias that invokes grilling and domain-modeling together, existing so a user can type one slash-command for the interview that leaves a paper trail in docs/glossary.md and docs/adr/. It stays disable-model-invocation, and the agent callers that want the same pairing - triage's grilling step and wayfinder's grilling ticket type - invoke grilling and domain-modeling directly rather than routing through the alias. No composition module goes between the primitives and their callers: the alias adds a hop without concentrating behavior, and the pairing it names is two skill names, not knowledge worth a module.

Recorded because an architecture review proposed making the composite agent-invocable so callers could invoke its interface instead of restating the two-skill recipe. The restatements are short mentions carrying call-site-specific instructions - triage tells the interview to sharpen domain terms as decisions land; wayfinder tells it to work breadth-first - so there is no duplicated protocol to concentrate.

## Consequences

- grill-me and grill-with-docs remain shallow by design: aliases are not modules, and a one-line implementation is their interface role.
- Agent-side pairing with the docs flow is spelled inline where it is used, and each restatement carries only what is specific to that call site.
