# chrisdigity skills

Language for this repository: a fork of Matt Pocock's engineering skills, restructured so the docs/ directory carries the whole agent workflow.

## Work in the tracker

**tracker**:
The local issue tracker at docs/tasks/, committed to git; it holds every feature spec and ticket until promotion or rejection.
_Avoid_: scratchpad, wip

**feature spec**:
The proposal document at docs/tasks/<feature>/spec.md, written by to-spec; it describes one feature's problem, user stories, and implementation decisions, and dies at promotion.
_Avoid_: wip spec, spec

**ticket**:
One tracer-bullet slice of a feature, with declared blocking edges, at docs/tasks/<feature>/issues/NN-slug.md.
_Avoid_: task, issue

**promotion**:
The close-out step that distills a completed feature into the durable docs; part of the feature's definition of done, checked by code-review.
_Avoid_: archive

**distillation**:
The routing rule promotion follows: behavior goes into the matching subsystem spec, hard-to-reverse decisions go into docs/adr/, new terms go into the glossary; everything else dies with the feature's directory.

## Reference docs

**subsystem spec**:
A current-state design intent document for one subsystem at docs/specs/<subsystem>.md; mutable, describing behavior as of the last promotion.
_Avoid_: spec

**precedence**:
When sources disagree: code and tests show what is, subsystem specs show what should be, the tracker shows what is proposed; surface the conflict rather than resolving it silently.

**research doc**:
A point-in-time, cited findings file at docs/research/<topic>.md; it is never promoted into a spec without re-verification.

**rejected-request KB**:
Dedup records at docs/out-of-scope/: one file per rejected enhancement concept with its prior requests; triage reads it before evaluating new requests, and an entry that passes the ADR bar graduates to docs/adr/ and leaves the KB.

**managed artifact**:
A file update-engineering-skills owns and regenerates: docs/agents/*.md and the writing-standard block in the steering file; everything else under docs/ is unmanaged and preserved as-is.

## Distribution

**upstream**:
Matt Pocock's original skill set, from which these skills derive; his updates are imported into this fork periodically, never the reverse. Divergences are recorded in the fork's README so each import stays deliberate.

**fork**:
A derived, independently maintained copy of upstream; updates flow from upstream into it, never back. This repository is the fork, installed into projects with npx skills add chrisdigity/skills.
_Avoid_: template repo

**steering file**:
The always-loaded agent instruction file in a repo: AGENTS.md, or CLAUDE.md where that is what already exists.

**writing standard**:
The versioned block in the steering file governing how all agent output reads; setup-engineering-skills writes it, update-engineering-skills replaces it in place.
