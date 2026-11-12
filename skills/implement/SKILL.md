---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

If the user passes a ticket reference, fetch it from the issue tracker and state its title before starting. If the reference is ambiguous, ask.

Call the Skill tool with "tdd" where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, call the Skill tool with "code-review" to review the work.

Commit your work to the current branch.

Then close out with promotion. The tracker holds what is proposed; the durable docs hold how the system should now behave, so a finished feature is distilled out of the tracker before its directory goes away:

- Distill the behavior into the matching subsystem spec at `docs/specs/<subsystem>.md`; if none covers the subsystem, create one named with the kebab-case glossary term for it, with the four sections (Purpose, Behavior, Interfaces and contracts, Decisions in force) and no feature slugs or proposed-work marks.
- Offer an ADR at `docs/adr/` for each hard-to-reverse decision the work made; write the ones the user confirms.
- Add new terms to the glossary at the location `docs/agents/domain.md` describes, `docs/glossary.md` by default.
- Resolve the tracker entries the way the issue tracker closes work.
- Remove the feature directory last, once its essence lives in the durable docs; git history retains the rest.

Commit the promotion edits. Promotion is done only when all five outcomes are checkable: specs updated, ADRs offered, glossary updated, tracker entries resolved, feature directory removed.
