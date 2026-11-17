---
name: setup-agent-docs
description: "Configure this repo for the engineering skills and reconfigure it any time: set up or update its issue tracker, triage label vocabulary, domain doc layout, research and disposable docs locations, git policy, and writing standard. Re-running asks the same questions together, with the current configuration as the defaults, offers seed drift as diffs, and preserves everything custom under docs/."
disable-model-invocation: true
---

# Setup agent docs

Configure the per-repo configuration that the engineering skills assume, and re-run at any time as a reinstall:

- **Issue tracker**: where issues live (local markdown under `docs/tasks/` by default; GitHub, GitLab, and freeform workflows also supported)
- **Triage labels**: the strings used for the five canonical triage roles
- **Domain docs**: where the glossary and ADRs live, and the rules for reading them
- **Research and disposable docs**: where research lands (`docs/research/`) and where disposable working files go (gitignored `docs/tmp/`)
- **Git**: what git operations agents may perform (pushing defaults to never on their own), and the protocol for a failed git operation
- **Writing standard**: the always-loaded block in the steering file that governs how all agent output reads

One pass serves every run. On a first run nothing is recorded, so the questions carry the setup defaults. On a re-run the same questions carry the currently recorded configuration as the recommended answers, so keeping the setup current is a one-word reply to a single batched message, and changing any part of it is answering that message's questions differently. Seed drift between the installed seed templates and the managed artifacts is offered as diffs. Nothing is inferred and no intent is assumed: the caller's answers decide every write.

This is a prompt-driven skill, not a deterministic script. Explore, present what you found, confirm with the user, then write.

Skill-file updates are the caller's job, not this skill's: installing or updating the skills themselves belongs to the skills CLI (`npx skills add` / `npx skills update`), and this skill runs no CLI step and never writes outside the managed artifacts. It reads the seed templates from its own folder as installed, so bringing the configuration level with new skills is two caller steps: update the skills, then run this skill.

## The managed artifacts

The managed contract: `docs/agents/*.md`, the steering file's two managed blocks (the `## Agent skills` block and the writing-standard block), and the `.gitignore` entry for the disposable docs directory are owned by this skill. Everything else under `docs/` is unmanaged and preserved as-is, so custom extensions survive untouched.

The seed templates live in this skill's folder; a managed artifact is written byte-identical to its seed unless the user's own words replace part of it:

- `issue-tracker-local.md`, `issue-tracker-github.md`, `issue-tracker-gitlab.md`, `issue-tracker-freeform.md`: the four tracker variants behind `docs/agents/issue-tracker.md`
- `domain-single-context.md`, `domain-multi-context.md`: the two variants behind `docs/agents/domain.md`, only the chosen one written
- `triage-labels.md`, `git-operations.md`: target `docs/agents/<seed file name>`
- `writing-standard.md`: targets the steering-file block

If the seed files are missing from this folder, the install is broken: say so and point at reinstalling the skills (`npx skills add chrisdigity/skills`); never regenerate a managed artifact from memory.

## Process

### 1. Explore

Look at the current repo to understand its starting state. Read whatever exists; don't assume:

- `git remote -v` and `.git/config`: is this a GitHub or GitLab repo? Which one?
- `AGENTS.md` and `CLAUDE.md` at the repo root: does either exist? Is there already an `## Agent skills` section or a delimited writing-standard block in either?
- `docs/agents/`: enumerate every file in it (the managed contract covers all of it, not just the seeds' targets) and classify each against this folder's seeds:
  - **Current**: matches its seed for that path.
  - **Drift**: same title and section structure as the seed, different content; the diff is presented in step 3.
  - **Conflict**: a custom file at a managed path (its title matches no seed for that path); raised side by side, never silently overwritten.
  - **Unattributable**: no seed explains it and no classification fits; raised and left alone unless the user says otherwise.
  - **Missing**: an expected artifact with no file.
- `docs/tasks/`: is the local tracker already in use? A `.scratch/` directory with feature folders is the predecessor convention; if you find one, offer to move its features into `docs/tasks/` with `git mv` before writing anything.
- `docs/glossary.md`, `docs/glossary-map.md`, `src/<context>/docs/glossary.md`, and the legacy root `GLOSSARY.md` / `GLOSSARY-MAP.md`: existing domain docs get recorded in the config, not re-scaffolded.
- `docs/adr/` and any `src/<context>/docs/adr/` directories.
- Is the `triage` skill installed? (a `triage` skill folder alongside this one, or `triage` in your available skills.) This decides whether Section B runs at all.
- Monorepo signals: a `pnpm-workspace.yaml`, a `workspaces` field in `package.json`, or a populated `packages/*` with its own `src/`. These are present only in a genuinely large multi-package repo; their absence means single-context, which is almost every repo.
- The steering file's writing-standard block, if present: read its version and compare to [writing-standard.md](./writing-standard.md); a difference is drift, presented in step 3.
- `.gitignore`: whether it carries the `docs/tmp/` entry.

### 2. Present findings and ask

Summarise what's present and what's missing, including the classification results. Then prepare every question the run needs and ask them all in one message, labeled by section: each question led by its recommended answer, so one reply settles the whole set, and accepting every recommendation can be a single word. Exploration settles everything it can first; the questions are prepared once, after it, and never asked one section per turn. When an answer opens follow-up questions (a hosted tracker without a matching remote, a freeform pick needing the workflow paragraph), ask those follow-ups batched in one message as well.

Lead each question with the recommended answer - the currently recorded configuration on a re-run, the setup default on a first run - so the user can accept it in a word. Give a one-line explainer only when the choice genuinely branches. Omit a question entirely when exploration already settled it (Section B when `triage` isn't installed, Section C when there's no monorepo and the user hasn't asked about it); state what was settled instead of asking it.

**Section A: Issue tracker.**

> Explainer: The "issue tracker" is where issues live for this repo. Skills like `to-tickets`, `triage`, and `to-spec` read from and write to it. They need to know whether to write a markdown file under `docs/tasks/`, call `gh issue create`, or follow some other workflow you describe. Pick the place you actually track work for this repo.

Default posture: the local markdown tracker at `docs/tasks/<feature>/` is the recommended default on a first run. Specs and tickets are committed to git, so work follows its branch and collaborators see it. When a tracker is already recorded, lead with it as the recommended answer and offer the alternatives. The options:

- **Local markdown**: issues live as files under `docs/tasks/<feature>/` in this repo (recommended default on a first run)
- **GitHub**: issues live in the repo's GitHub Issues (uses the `gh` CLI)
- **GitLab**: issues live in the repo's GitLab Issues (uses the [`glab`](https://gitlab.com/gitlab-org/cli) CLI)
- **Other** (Jira, Linear, etc.): ask the user to describe the workflow in one paragraph; the freeform template records it as prose

Record the choice in `docs/agents/issue-tracker.md`, rewriting the file from the chosen seed; switching variants rewrites the file. The GitHub and GitLab templates carry a "PRs as a request surface" flag, defaulted **off**. Leave it off and don't raise it: a user who wants external PRs in the triage queue can flip the flag in the file later.

When the current tracker is freeform, the recommended answer keeps the recorded workflow. Re-dictating the paragraph rewrites the file's workflow text in the user's new words, carried in verbatim, never paraphrased: the freeform file's workflow text is the user's own words, replaced only by their answer here.

A GitHub or GitLab tracker needs a reachable repo. When the user picks one and no `git remote` points at that host, get the repo URL from them and wire it up (`git init` if needed, `git remote add origin <url>`) before going further. Done when `gh repo view` / `glab repo view` prints the project from inside this directory; if it can't, stop and tell the user what's missing.

**Section B: Triage label vocabulary.** Skip this section entirely if the `triage` skill isn't installed (exploration told you), since an uninstalled skill needs no labels.

If it is installed, ask:

> Do you want to keep the default triage labels? (a re-run leads with the recorded vocabulary)

The defaults are the five canonical roles, each label string equal to its name: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. Keeping them, or keeping a previously recorded vocabulary, changes nothing unless the file has drifted from its seed. Only if the user overrides, usually because their tracker already uses other names (e.g. `bug:triage` for `needs-triage`), collect the overrides so `triage` applies existing labels instead of creating duplicates.

**Section C: Domain docs.** Default to **single-context** (one `docs/glossary.md` + `docs/adr/`). This fits almost every repo; write it without asking. If the user asks for multi-context directly, honor that request.

Offer **multi-context** (a root `docs/glossary-map.md` pointing at per-context glossaries at `src/<context>/docs/glossary.md`) only when exploration found monorepo signals, or when the user asks about it. Then confirm which layout they want. The variant is never switched without the user's answer saying so.

Only the chosen variant is written: `docs/agents/domain.md` describes the layout this repo actually uses, never both.

**Section D: Git policy.** The user decides whether this repo carries it at all, and the section asks on every run - as one compound question in the batch, so one reply settles both parts:

> Do you want the git policy reference? It carries the push guardrails, generalized commit-requirement guidance, and the protocol for a failed git operation. (recommended: **yes**) If yes: may agents push to the remote on their own? (recommended: **no**)

When the policy is already adopted, the recommended answer keeps it as recorded: the file and Push line stay unless the user answers differently or adopts a drift diff. When it was declined before (no `### Git operations` sub-block and no file), this is the same question re-offered: a decline writes nothing and leaves no record, so the next run asks again.

On **no** to the policy, write nothing: omit the `### Git operations` sub-block and don't create `docs/agents/git-operations.md`. On **yes**, the push answer from the same reply decides the Push line: **no** (the default) writes the seed as-is - agents never push on their own; the maintainer pushes, or explicitly asks for it; **yes** replaces the seed's Push line with what the user allowed, including any conditions they state (e.g. "only feature branches"). On a re-run, the recorded push answer is the recommended one, and keeping it leaves the Push line and any maintainer recordings untouched; changing it rewrites the Push line from the new answer. Omit the push sub-question when the repo has no remote; keep the default and say so. The other guardrails (force push, shared-history rewrites) ship as defaults in the seed; don't ask about each one - point the user at the file to edit them later.

The "Commit requirements" section ships generalized, never prefilled from the setup machine's git config: requirements sit with each maintainer's machine, so one maintainer's signing setup does not describe another's. If the repo enforces something for everyone (branch protection demanding signed commits, a CI sign-off check), the maintainer records it in the file later. Step 3 shows the draft, so the user can correct it before anything is written.

Write the `### Git operations` one-liner in the same pointer style as the other entries: one short line stating the push answer and the reporting rule, never a copy of the file's guidance:

> Agents never push on their own; a failed git operation is reported to the maintainer before diagnosis. See `docs/agents/git-operations.md`.

> Agents may push to the remote; a failed git operation is reported to the maintainer before diagnosis. See `docs/agents/git-operations.md`.

**Beyond the sections.** These inventory items are presented in the plan rather than asked as questions:

- **Seed drift**: a managed artifact whose title and section structure match its seed but whose content differs; show the diff and offer to regenerate byte-identical. The same offer covers the writing-standard block, which is located by its delimiters and replaced in place; hand edits to the block are overwritten by design, and custom extensions belong in unmanaged files.
- **Freeform seed drift**: when the freeform seed changed, present the seed's diff; applying it re-renders the file around the user's recorded workflow text, carried over verbatim.
- **Conflicts**: a custom file at a managed path is presented side by side with the managed artifact it shadows; the user keeps theirs (the path stays custom and unmanaged from here on) or adopts the managed artifact (theirs is moved aside first, never deleted).
- **Unattributable files**: left alone unless the user says otherwise.
- **Newly managed artifacts**: a seed the current managed set does not know is offered; adopting it creates the file and extends the steering file's `## Agent skills` block to reference it.
- **Missing artifacts**: an expected `docs/agents/` file absent while its section's answer implies it exists is recreated from its seed.
- **`.gitignore`**: append the `docs/tmp/` entry when missing; create `.gitignore` with just that entry when the repo has none.

### 3. Confirm and edit

Show the user a draft of:

- The `## Agent skills` block to add to (or update in) whichever of `CLAUDE.md` / `AGENTS.md` is being edited (see step 4 for selection rules)
- The writing-standard block from [writing-standard.md](./writing-standard.md), and the drift diff offered for the installed block when one exists. The delimiters and version are part of the block's contract, so its wording is not edited here: wording changes ship only after an approved change list.
- The contents (or diffs) of every `docs/agents/` file the plan touches: `issue-tracker.md` and `domain.md` always; `triage-labels.md` only when `triage` is installed; `git-operations.md` only when the user accepted Section D or adopted its drift

Let them edit before writing. State the preservation guarantee plainly: nothing outside `docs/agents/`, the steering file's two managed blocks, and the `.gitignore` `docs/tmp/` entry will be written.

### 4. Write

**Pick the file to edit:**

- If `CLAUDE.md` exists, edit it.
- Else if `AGENTS.md` exists, edit it.
- If neither exists, ask the user which one to create; don't pick for them.

Never create `AGENTS.md` when `CLAUDE.md` already exists (or vice versa); always edit the one that's already there.

If an `## Agent skills` block already exists in the chosen file, update its contents in-place rather than appending a duplicate. Don't overwrite user edits to the surrounding sections.

The block:

```markdown
## Agent skills

### Domain docs

[one-line summary of layout: "single-context" or "multi-context"]. See `docs/agents/domain.md`.

### Issue tracker

[one-line summary of where issues are tracked]. See `docs/agents/issue-tracker.md`.

### Triage labels

[one-line summary of the label vocabulary]. See `docs/agents/triage-labels.md`.

### Research docs

Research lives under `docs/research/`, one file per topic. Cross-reference files as `docs/research/<topic>` in ADRs, specs, and tickets.

### Disposable docs

Disposable working files (drafts, temp notes, scratch comparisons) go in gitignored `docs/tmp/`. Nothing committed references a `docs/tmp/` path.

### Git operations

[one-line summary of git policy and the failed-operation protocol]. See `docs/agents/git-operations.md`.
```

Include the `### Triage labels` sub-block, and write `docs/agents/triage-labels.md`, only when `triage` is installed and Section B ran. When it isn't, both are omitted. Include the `### Git operations` sub-block, and write `docs/agents/git-operations.md`, only when the user accepted Section D; when they didn't, both are omitted.

Then ensure `.gitignore` carries the disposable docs entry: append a `docs/tmp/` line when it is missing, creating `.gitignore` with just that entry when the repo has none.

Then write the writing-standard block from [writing-standard.md](./writing-standard.md) verbatim, delimiters included, into the steering file below the agent-skills block. If a delimited writing-standard block already exists there, locate it by its delimiters and replace its contents - version and delimiters included - rather than appending a second block; never touch the rest of the steering file.

When Section B ran on GitHub or GitLab, create each configured label the tracker lacks (`gh label create` / `glab label create`).

Then write the docs files byte-identical to the seed templates in this skill folder, unless the user's words replace part of them (a freeform workflow paragraph, a push answer with conditions):

- [issue-tracker-local.md](./issue-tracker-local.md): local-markdown issue tracker (the default)
- [issue-tracker-github.md](./issue-tracker-github.md): GitHub issue tracker
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md): GitLab issue tracker
- [issue-tracker-freeform.md](./issue-tracker-freeform.md): any other workflow, filled in from the user's description
- [triage-labels.md](./triage-labels.md): label mapping (only if `triage` is installed)
- [domain-single-context.md](./domain-single-context.md) or [domain-multi-context.md](./domain-multi-context.md), written to `docs/agents/domain.md`: only the chosen variant
- [git-operations.md](./git-operations.md): git policy (with the user's push answer) and the failed-operation protocol (only if the user accepted Section D)

When a conflict's resolution was "adopt the managed artifact", move the user's file aside first (never delete it), then write the managed artifact.

### 5. Done

Report what was kept, what changed, what was created, what was preserved, and how each conflict, unattributable file, and drift offer was resolved. Done when all three are checkable:

- Every managed artifact ends in the state the confirmed plan defined: matching its seed, carrying the user's words where an answer replaced them, or under a documented exception (conflict kept custom, unattributable left alone).
- Unmanaged content untouched: `git status` shows changes only at the expected paths.
- Every conflict, drift offer, and newly managed artifact got an explicit resolution from the user.

Close by noting that re-running this skill at any time repeats the same batched questions with the recorded answers as the defaults, and that skill updates come from the skills CLI: after an update, run this skill again to bring the configuration level with the new seeds.
