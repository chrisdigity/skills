# chrisdigity skills

Language for this repository: a fork of Matt Pocock's engineering skills, restructured so the docs/ directory carries the whole agent workflow.

## Work in the tracker

**tracker**:
The local issue tracker at docs/tasks/, holding every feature's spec, tickets, and research until cleanup or rejection. In a configured repo it lives in the repo's docs/ and is committed to git; on an unconfigured repo it lives under the fallback root and is outside git.
_Avoid_: scratchpad, wip

**fallback root**:
The stand-in repository root skills use when a repo has no agent-docs configuration: %TEMP%/<repo-slug>/ in the OS temp directory, with every docs/ convention held verbatim beneath it (tasks/, tmp/, out-of-scope/). Disposable, never written into the real repo, never the home of domain docs.
_Avoid_: temp tracker root, external tracker

**fallback tracker**:
The local markdown tracker under the fallback root at docs/tasks/, identical in layout to the configured tracker but outside git.
_Avoid_: temp tracker, session tracker

**feature spec**:
The proposal document at docs/tasks/<feature>/spec.md, written by to-spec; it describes one feature's problem, user stories, and implementation decisions, and is removed by cleanup when the feature finishes.
_Avoid_: wip spec, spec

**ticket**:
One tracer-bullet slice of a feature, with declared blocking edges, at docs/tasks/<feature>/issues/NN-slug.md.
_Avoid_: task, issue

**wayfinding**:
The map-and-ticket system wayfinder runs over the tracker: a map file with one child ticket per question, claimed and resolved by status, with the frontier listing open, unblocked, unclaimed tickets.

**cleanup**:
The skill that removes finished work's state from the tracker: a feature directory removed wholesale once all its tickets resolve, its research included; git history retains what the workspace drops, and the glossary and ADRs written along the way are the durable record.
_Avoid_: promotion, archive

## Reference docs

**research doc**:
A point-in-time, cited findings file. Unadopted research is an untracked working file at docs/tmp/<topic>.md - in a configured repo, under the fallback root in an unconfigured one; research a feature consumes moves into docs/tasks/<feature>/research/ and is removed with the feature's state. Durable documents receive distilled findings, never research paths.

**disposable docs**:
Working files at docs/tmp/ that exist only for the current task: drafts, temp notes, unadopted research, and the upstream import's comparison install. Gitignored in a configured repo, under the fallback root in an unconfigured one; deleted without notice, never referenced by committed documents.
_Avoid_: scratchpad

**rejected-request KB**:
Dedup records at docs/out-of-scope/: one file per rejected enhancement concept with its prior requests; triage reads it before evaluating new requests, and an entry that passes the ADR bar graduates to docs/adr/ and leaves the KB.

**managed artifact**:
A file setup-agent-docs owns and regenerates: docs/agents/*.md and the steering file's two managed blocks (the agent-skills block and the writing-standard block), plus the .gitignore entry for the disposable docs directory, which the skill verifies for presence rather than regenerating; everything else under docs/ is unmanaged and preserved as-is.

**seed**:
The template file in the setup-agent-docs folder that a managed artifact is generated from; the skill classifies each managed artifact against the installed seed and regenerates byte-identical copies of it.

## Distribution

**upstream**:
Matt Pocock's original skill set, from which these skills derive; his updates are imported into this fork periodically, never the reverse. Divergences are recorded in the fork's README so each import stays deliberate.

**divergence list**:
The curated list of every live divergence from upstream, one entry per divergence with its reason, maintained in place; a carried skill's edit updates its divergence's entry or adds one. It is what makes each upstream import a deliberate act rather than a silent overwrite. _Avoid_: import log, changelog.

**fork**:
A derived, independently maintained copy of upstream; updates flow from upstream into it, never back. This repository is the fork, installed into projects with npx skills add chrisdigity/skills.
_Avoid_: template repo

**steering file**:
The always-loaded agent instruction file in a repo: AGENTS.md, or CLAUDE.md where that is what already exists.

**writing standard**:
The versioned block in the steering file governing how all agent output reads; setup-agent-docs writes it and replaces it in place on re-runs.
