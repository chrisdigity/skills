# Skills distribution

## Purpose

Skills distribution is the subsystem that carries the skill set from upstream through this fork into installed projects. It covers what the fork packages, how a project installs it, and how upstream changes arrive. Project configuration and the work lifecycle are separate subsystems with their own specs.

## Behavior

The repository is a fork of upstream: a derived, independently maintained copy. Updates flow from upstream into the fork, never the reverse, and nothing is contributed back.

Skills are packaged one directory per skill, each directory holding the skill's definition file with its name and description frontmatter plus any reference files, scripts, and agent metadata. The layout sits within the three-level depth the skills CLI discovers. Skill names are kept from upstream so upstream changes stay visible during imports; the only rename is the router, upstream's ask-matt, which is ask-skills here. One upstream skill is not carried: setup-matt-pocock-skills is superseded by this fork's setup-engineering-skills.

A project installs the set with the skills CLI, project-locally by default or globally with a flag; the CLI requires Node.js 22.20 or later. The install writes a lockfile that records content hashes for every installed skill folder, so an installed copy is verifiable against the source without rerunning the install.

Every deliberate divergence from upstream is listed, with its reason, in the divergence list, one entry per live divergence; a carried skill's edit updates its divergence's entry or adds one. The list is what makes each upstream import a deliberate act rather than a silent overwrite: the importer can tell upstream's new work from a divergence to defend without archaeology.

## Interfaces and contracts

- Install contract: one command into any project, naming this repository, installs every skill the CLI discovers here; installed folders are byte-identical copies of the source folders.
- Verification contract: the lockfile's computed hashes identify which installed folders differ from their source, so drift from hand edits or partial updates is detectable.
- Divergence contract: one entry per live divergence, stating what differs and why, maintained in place rather than appended to; the file's length is bounded by the divergence count, not the edit count.
- Consumer contract: the project-configuration subsystem's update skill refreshes installed skill folders through the CLI and reads its seed templates from the installed sibling folder, so the install layout is what migration operates on.

## Decisions in force

- ADR 0004, skill-file refresh delegates to the skills CLI: installed copies are refreshed through the CLI's check and update path, and the lockfile's hashes stay the install-state truth; no parallel installer exists.
- No ADR governs upstream derivation. The one-directional policy is operating policy recorded in the divergence list, deliberately not standardized as a decision record.
