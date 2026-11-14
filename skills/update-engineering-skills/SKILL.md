---
name: update-engineering-skills
description: "Refresh the installed engineering skills to the latest version through the skills CLI, then migrate this repo's managed configuration (docs/agents/*.md and the steering file's writing-standard block) to the latest templates. Preserves everything custom under docs/."
disable-model-invocation: true
---

# Update engineering skills

Run this after the engineering skills were installed or updated. It has two jobs, in order:

1. **Refresh the skill files**, by delegating to the skills CLI.
2. **Migrate the managed artifacts**, so this repo's configuration matches the refreshed skills.

The managed-artifacts contract: `docs/agents/*.md` and the writing-standard block in the steering file are owned by `setup-engineering-skills` and this skill. Everything else under `docs/` is unmanaged and preserved as-is, so custom extensions survive an update untouched.

This skill is not an installer. Skill-file refresh goes through the CLI; this skill's own instructions are spent entirely on migration.

## Process

### 1. Refresh the skill files (the CLI's job)

From the repo root:

- `npx skills check`: reports skills with available updates, plus installed skill files whose content differs from the hash in `skills-lock.json` ("has local changes"). Exit code 1 means there is something to update; exit code 0 means everything is current.
- `npx skills update`: refreshes installed skills from their source. Add `-y` to skip the scope prompt (auto-detects project scope in a project directory), or `-g` instead when the skills were installed globally. Name skills after the command to update only those.

Read the check output before updating. A "has local changes" entry is an installed skill file someone edited by hand, and the update reinstalls it from the source repo, losing that edit: flag each one to the user before running the update.

If `npx skills check` exits 0, the files are current and the update is skipped; continue to migration, since the managed artifacts may have drifted on their own.

Do not copy skill files by hand, write your own installer, or patch installed `SKILL.md` files to simulate an update. If the CLI errors, report the error and stop.

### 2. Locate the seed templates

Managed artifacts are regenerated from the seed templates in the `setup-engineering-skills` folder installed next to this skill's folder (project installs: `.agents/skills/setup-engineering-skills/`; global installs: the same folder under the global skills directory). Step 1 refreshed it, so its seeds are the latest:

- `issue-tracker-local.md`, `issue-tracker-github.md`, `issue-tracker-gitlab.md`, `issue-tracker-freeform.md`: the four tracker variants
- `domain-single-context.md`, `domain-multi-context.md`: the two domain-docs variants
- `triage-labels.md`: the label mapping
- `git.md`: the git policy reference
- `writing-standard.md`: the versioned writing-standard block

If the sibling folder is missing (a partial install), stop and tell the user to reinstall the pair with `npx skills add chrisdigity/skills`. Never regenerate a managed artifact from memory. When the skills were installed globally, `npx skills list -g` prints the global skills directory this folder lives under.

### 3. Inventory the managed artifacts

If `docs/agents/` does not exist and no delimited writing-standard block exists in any steering file, setup has never run in this repo: say so and point at `/setup-engineering-skills`; there is nothing to migrate.

Otherwise enumerate every file under `docs/agents/` (the managed contract covers all of it, not just the paths below), then read:

- `docs/agents/issue-tracker.md`: the tracker type, from its title ("Issue tracker: local markdown" / GitHub / GitLab / freeform). When the steering file's `## Agent skills` block is present, cross-check its `### Issue tracker` one-liner; on a mismatch, surface it in the plan instead of picking one silently.
- `docs/agents/domain.md`: the variant, from its File structure section. Never switch variants during an update.
- `docs/agents/triage-labels.md`: present or missing (managed only while the `triage` skill is installed).
- `docs/agents/git.md`: present or missing (managed only when the steering file's agent-skills block carries a `### Git` sub-block; its Push line records the repo's push policy).
- The steering file (`CLAUDE.md` where that is what exists, else `AGENTS.md`): the delimited writing-standard block and the version in its opening delimiter (`<!-- writing-standard v1: ... -->`). If neither steering file exists, there is nothing to migrate there.

Classify each existing artifact against the latest seed for its path. The seed-to-path mapping comes from the sibling folder's file names: `issue-tracker-*` and `domain-*` are the variants behind `issue-tracker.md` and `domain.md`, `triage-labels.md` targets itself, `writing-standard.md` targets the steering-file block, and any other seed file targets `docs/agents/<seed file name>`, so a seed the current managed set does not know is a newly managed artifact:

- **Matches the seed**: current, nothing to do.
- **Same shape, different content** (title and section structure match the seed's): a managed artifact that is outdated or hand-edited. The migration regenerates it; show the diff in the plan. For the writing-standard block this overwrite is by design: hand edits to the block are lost, and custom extensions belong in unmanaged files.
- **Different shape** (its title matches no seed for that path): not setup output. A custom file is sitting at a managed path; raise a conflict (step 4) and never overwrite it. This is what catches a custom file shadowing an artifact the latest templates newly manage.
- **freeform**: an `issue-tracker.md` written from the freeform seed is never regenerated, whatever it now contains; its workflow text is the user's own words. If the freeform seed changed, present the seed's diff and ask how to proceed.
- **user-owned**: `git.md` is never regenerated once written; its Push answer and any maintainer recordings are the user's own words. If the git seed changed, present the seed's diff and ask how to proceed.
- **Missing**: offer to create it from the latest seed in the plan.

A file under `docs/agents/` that no seed explains and no classification above fits is unattributable: raise it in the plan and leave it alone unless the user says otherwise.

### 4. Present the migration plan and confirm

Present the plan before any write, one line per action:

- What step 1 did (updated, or already current).
- Per managed artifact, the action its step 3 classification calls for: regenerate (with the diff), already current, create, preserve, or raise as a conflict.
- Each conflict and its proposed resolution. A custom file at a managed path is presented side by side with the managed artifact it shadows, and the user picks: keep theirs (the path stays custom and this skill no longer manages it), or adopt the managed artifact (theirs is moved aside first, never deleted). When the latest templates introduce a managed artifact this repo does not have, offer to create it and extend the steering file's `## Agent skills` block to reference it; the acceptance-gated git policy is the exception, offered only when its `### Git` sub-block is already present and otherwise left to setup.
- The preservation guarantee: nothing outside `docs/agents/` and the steering file's two managed blocks (the `## Agent skills` block and the writing-standard block) will be written.

Wait for an explicit yes. No write happens before it.

### 5. Write

- Regenerate each confirmed artifact byte-identical to its seed.
- Replace the writing-standard block in place: locate it by its delimiters, then swap the contents and version for the latest seed's block, delimiters included. When the block is missing, insert the latest one below the `## Agent skills` block. Never touch the rest of the steering file.
- Preserve the freeform tracker file and the user-owned git.md as-is.

### 6. Done

Report what the CLI refreshed, what migrated, what was preserved, and how each conflict was resolved. End by noting that optional managed artifacts the latest templates carry but this repo has not adopted (currently `git.md`) can be added by re-running `/setup-engineering-skills`. Done when all four are checkable:

- `npx skills list` shows no `modified` skills (installed files match the lock's content hashes)
- Every managed artifact ends in the state its step 3 classification and the confirmed plan defined: matching its latest seed, or under a documented exception (freeform preserved, git.md preserved, conflict kept custom)
- Unmanaged content untouched: `git status` shows changes only at the expected paths
- Every conflict raised got an explicit resolution from the user
