# Project configuration

## Purpose

Project configuration is the subsystem that prepares a repository to run the workflow and keeps that configuration current: one skill writes it, one migrates it, and everything the pair does not own is left untouched. It covers the setup skill's write path, the update skill's refresh and migration path, and the ownership line between managed and unmanaged files.

## Behavior

setup-engineering-skills configures a repository through a prompt-driven process: explore, present, confirm, write. It configures the issue tracker, the triage labels, the domain-docs variant, and the steering file's two blocks. The local markdown tracker is the recommended default, proposed first; GitHub and GitLab trackers are the alternatives when the user prefers a hosted one, and a freeform tracker is available where the user dictates the workflow in their own words. The domain docs are emitted in exactly one variant, single-context by default and multi-context when the repository's shape or the user's request calls for it, so unused context does not ship. The writing-standard block's wording is not edited at setup time; wording changes ship only through an approved change list, per ADR 0001.

update-engineering-skills delegates file refresh to the skills CLI: check first, then update, flagging skills with local changes before the update overwrites their hand edits. The skill implements no installer. Its own work is migration: it inventories every managed artifact, classifies each against the latest seed as regenerate, conflict, preserve, or missing-and-offered, presents a plan carrying the preservation guarantee, and writes only after an explicit yes. Regeneration is byte-identical to the seed. The writing-standard block is replaced in place, located by its delimiters. A custom file at a managed path is raised as a conflict, never overwritten; the freeform tracker's workflow text is the user's own words and is never regenerated.

Migration reads the seeds from the installed setup-engineering-skills folder the CLI refreshed in the same run; a missing sibling folder stops the run with a reinstall pointer rather than regenerating from memory, so the pair ships together.

## Interfaces and contracts

- The managed-artifacts contract: the per-topic configuration files and the steering file's writing-standard block are owned by setup and update; every other file in the documentation directory is unmanaged and preserved as-is (ADR 0003).
- Single source of truth: the seed templates live in the setup skill's folder and update keeps no copies of its own, so the CLI's refresh of that folder is what makes the next migration current.
- The writing-standard block is versioned and delimited so update can locate and replace it by version without touching the steering file's other content.
- Location truth: the per-topic configuration files are where other skills read the tracker's path, the labels, and the domain-docs variant; carried skills defer to that configuration instead of hardcoding locations.

## Decisions in force

- ADR 0001, the writing standard ships as a managed steering-file block: the block is always-loaded context on turn one; wording changes ship only after an approved change list, and update replaces the block in place.
- ADR 0003, setup and update own the managed artifacts: ownership is unambiguous because update regenerates owned paths in place, so custom extensions belong in unmanaged files.
- ADR 0004, skill-file refresh delegates to the skills CLI: no parallel installer; the lockfile's hashes are the install-state truth.
