# Project configuration

## Purpose

Project configuration is the subsystem that prepares a repository to run the workflow and keeps that configuration current: one skill both writes and migrates it, and everything the skill does not own is left untouched. It covers the skill's write path, its re-run path, and the ownership line between managed and unmanaged files.

## Behavior

setup-agent-docs configures a repository through a prompt-driven process: explore, present, confirm, write. It configures the issue tracker, the triage labels, the domain-docs variant, the research and disposable docs locations, the git policy, and the steering file's two blocks. Every run asks the same questions in one batched message: on a first run nothing is recorded, so the questions carry the setup defaults; on a re-run they carry the currently recorded configuration as the recommended answers, so keeping the setup current is a one-word reply, and changing any part of it - the tracker variant, the push answer, the freeform workflow text - is answering the question differently. Seed drift between the installed seed templates and the managed artifacts is presented as diffs and applied byte-identical only by explicit choice.

The local markdown tracker is the recommended default on a first run; GitHub and GitLab trackers are the alternatives when the user prefers a hosted one, and a freeform tracker records the user's workflow in their own words, replaced only when they re-dictate it. The domain docs are emitted in exactly one variant, single-context by default and multi-context when the repository's shape or the user's request calls for it, and the variant is never switched without the user's answer saying so. The git policy ships only when the user accepts it: a decline writes nothing and leaves no record, so the section re-offers it on the next run, and the steering block's `### Git operations` sub-section records acceptance. The file's Push line and any maintainer recordings are rewritten only from the user's answers; seed changes reach its generalized guidance as a presented diff, adopted by explicit choice. The skill never prefills commit requirements from the setup machine's git config: requirements sit with each maintainer's machine, so one maintainer's signing setup does not describe another's. The skill also ensures `.gitignore` carries the `docs/tmp/` entry for disposable working files, appending it when missing. The writing-standard block's wording is not edited at write time; wording changes ship only through an approved change list, per ADR 0001.

The skill implements no installer and runs no skills-CLI step: installing or updating skills is the caller's CLI job, and the skill reads the seed templates from its own installed folder as they are, so a migration against the installed seeds is version-coherent with the installed skills. A custom file at a managed path is raised as a conflict, never overwritten; a file no seed explains is raised as unattributable and left alone unless the user says otherwise; a seed the managed set does not know is a newly managed artifact, offered together with the steering-block pointer that references it. Regeneration is byte-identical to the seed unless the user's words replace part of the file. The writing-standard block is replaced in place, located by its delimiters. Everything outside `docs/agents/`, the steering file's two managed blocks, and the `.gitignore` `docs/tmp/` entry is never written.

## Interfaces and contracts

- The managed-artifacts contract: the per-topic configuration files and the steering file's two managed blocks (the agent-skills block and the writing-standard block) are owned by setup-agent-docs; every other file in the documentation directory is unmanaged and preserved as-is (ADR 0003, consolidated by ADR 0007).
- Single source of truth: the seed templates live in the skill's own folder, so what the CLI installs is what the next run classifies against.
- The writing-standard block is versioned and delimited so the skill can locate and replace it by version without touching the steering file's other content.
- Location truth: the per-topic configuration files are where other skills read the tracker's path, the labels, the domain-docs variant, and the git policy; carried skills defer to that configuration instead of hardcoding locations.

## Decisions in force

- ADR 0001, the writing standard ships as a managed steering-file block: the block is always-loaded context on turn one; wording changes ship only after an approved change list, and the skill replaces the block in place.
- ADR 0003, the managed artifacts are owned: ownership is unambiguous because the skill regenerates owned paths in place, so custom extensions belong in unmanaged files. ADR 0007 consolidates ownership into one skill.
- ADR 0004, skill-file refresh delegates to the skills CLI: the configuration skill implements no installer and runs no CLI step; the lockfile's hashes are the install-state truth the caller maintains.
- ADR 0005, the git policy is acceptance-gated and user-owned: the steering block's `### Git operations` sub-section is the acceptance record, and the file is never silently rewritten; ADR 0007 supersedes the ends-at-adoption asking rule with a re-ask whose default is the recorded answer.
- ADR 0007, one skill configures and migrates: update-agent-docs is absorbed; a re-run is an install/reinstall walk.
