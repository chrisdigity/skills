# Divergences from upstream

This fork derives from mattpocock/skills, imported at upstream commit 4ae2317 on 2026-11-12. Derivation is one-directional: upstream updates are imported here periodically, never the reverse. This file lists every live divergence with its reason, one entry per divergence. A skill edit updates its divergence's entry or adds one, and an entry is removed only if the fork re-adopts upstream's behavior. The list is what makes each import a deliberate act: at import time the fork's differences are already named, so an upstream diff sorts into his new work, a divergence to defend, or drift to undo.

## The fork's shape

- The docs/ directory carries the whole agent workflow (tracker, subsystem specs, glossary, ADRs, research, rejected-request KB), committed to git. Upstream keeps the glossary at a root GLOSSARY.md, issues in a dot-prefixed .scratch/ directory, and the rejected-request KB in .out-of-scope/, all invisible to a gitignore that excludes dot-prefixed paths.
- Skill folders sit flat at skills/<name>/ instead of upstream's skills/<category>/<name>/, and upstream's category READMEs are not carried. The skills CLI discovers skills up to three levels deep, so the flat layout installs the same way.
- Skill names are kept from upstream so upstream changes stay visible during imports. The only rename is the router: ask-matt is ask-skills here, rewritten for the docs/ flow; PHASE-BOUNDARIES.md carried unchanged.
- setup-matt-pocock-skills is not carried; setup-engineering-skills supersedes it.

## Carried skills, edited

- domain-modeling: the file structure section defers to docs/agents/domain.md (defaults docs/glossary.md and docs/adr/; multi-context variant docs/glossary-map.md and src/<context>/docs/glossary.md) instead of inferring from a root GLOSSARY.md or GLOSSARY-MAP.md; GLOSSARY-FORMAT.md carries the same paths, and ADR-FORMAT.md points multi-context ADR locations at docs/agents/domain.md.
- to-tickets: local tickets go under the tracker path the issue-tracker config defines, defaulting to docs/tasks/<feature-slug>/issues/, instead of .scratch/<feature-slug>/issues/, so tickets are committed and follow their branch.
- triage: the rejected-request KB moved from .out-of-scope/ to docs/out-of-scope/, so rejections are tracked by git; OUT-OF-SCOPE.md and AGENT-BRIEF.md updated to match.
- research: findings save to docs/research/<topic>.md, one file per topic and dated, instead of wherever the agent guesses.
- tdd: the glossary line reads the project's glossary at the location docs/agents/domain.md describes, docs/glossary.md by default.
- improve-codebase-architecture: the glossary path follows docs/agents/domain.md, ADRs stay at docs/adr/, and a matching subsystem spec at docs/specs/ is orientation input for the scan.
- implement and implement-spec: a promotion close-out step added after code-review, making promotion part of done (upstream has none): behavior distills into the matching subsystem spec, hard-to-reverse decisions are offered as ADRs, new terms enter the glossary, tracker entries resolve, and the feature directory is removed last. Carries the governance model in ADR 0002.
- code-review: the spec-source search names the tracker's feature spec and adds docs/specs/<subsystem>.md as the source for promoted changes whose feature directory promotion removed; the Spec sub-agent also receives the covering subsystem specs' paths, and its brief gains the promotion-consistency check.
- to-spec: a cross-feature dependency rule added: exploration checks the tracker's open features, the spec template gains a Cross-feature dependencies section, and ticketing wires each dependency as a ticket blocking edge; the publish target names the tracker path.
- Six carried skills' pointers updated from /setup-matt-pocock-skills to /setup-engineering-skills: to-tickets (2 sites), to-spec, implement-spec, code-review, triage, wayfinder.

## Fork-authored skills

- setup-engineering-skills: new, superseding upstream's setup skill. Same prompt-driven process, restructured for this fork: the local markdown tracker is the recommended default (upstream proposed GitHub first), the local tracker seed is rewritten for the committed docs/tasks/ layout with wayfinding operations, a freeform tracker seed is added, docs/agents/domain.md is emitted in a single- and a multi-context variant with only the chosen one written, the triage-labels table's column header is unbranded ("Canonical role"), and a versioned, delimited writing-standard block (v2) is written into the steering file. The GitHub and GitLab tracker seeds are carried from upstream byte-identical.
- update-engineering-skills: new; upstream has no counterpart. Implements no installer: file refresh delegates to the skills CLI, and the skill's own instructions migrate the managed artifacts (inventory, classify against the latest seeds, plan and confirm before any write, regenerate byte-identical, preserve everything unmanaged, raise conflicts instead of overwriting). Ownership is recorded in ADR 0003.
