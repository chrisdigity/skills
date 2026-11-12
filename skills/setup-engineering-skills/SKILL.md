---
name: setup-engineering-skills
description: "Configure this repo for the engineering skills: set up its issue tracker, triage label vocabulary, domain doc layout, and writing standard. Run once before first use of the other engineering skills."
disable-model-invocation: true
---

# Setup engineering skills

Scaffold the per-repo configuration that the engineering skills assume:

- **Issue tracker**: where issues live (local markdown under `docs/tasks/` by default; GitHub, GitLab, and freeform workflows also supported)
- **Triage labels**: the strings used for the five canonical triage roles
- **Domain docs**: where the glossary and ADRs live, and the rules for reading them
- **Writing standard**: the always-loaded block in the steering file that governs how all agent output reads

This is a prompt-driven skill, not a deterministic script. Explore, present what you found, confirm with the user, then write.

## Process

### 1. Explore

Look at the current repo to understand its starting state. Read whatever exists; don't assume:

- `git remote -v` and `.git/config`: is this a GitHub or GitLab repo? Which one?
- `AGENTS.md` and `CLAUDE.md` at the repo root: does either exist? Is there already an `## Agent skills` section or a delimited writing-standard block in either?
- `docs/agents/`: does this skill's prior output already exist? If it does, this is a re-run: update the existing files in place rather than re-asking settled questions.
- `docs/tasks/`: is the local tracker already in use? A `.scratch/` directory with feature folders is the predecessor convention; if you find one, offer to move its features into `docs/tasks/` with `git mv` before writing anything.
- `docs/glossary.md`, `docs/glossary-map.md`, `src/<context>/docs/glossary.md`, and the legacy root `GLOSSARY.md` / `GLOSSARY-MAP.md`: existing domain docs get recorded in the config, not re-scaffolded.
- `docs/adr/` and any `src/<context>/docs/adr/` directories.
- Is the `triage` skill installed? (a `triage` skill folder alongside this one, or `triage` in your available skills.) This decides whether Section B runs at all.
- Monorepo signals: a `pnpm-workspace.yaml`, a `workspaces` field in `package.json`, or a populated `packages/*` with its own `src/`. These are present only in a genuinely large multi-package repo; their absence means single-context, which is almost every repo.

### 2. Present findings and ask

Summarise what's present and what's missing. Then take the sections in order. One section, one answer, then the next.

Lead each section with the recommended answer so the user can accept it in a word. Give a one-line explainer only when the choice genuinely branches; skip the section entirely when exploration already settled it (Section B when `triage` isn't installed, Section C when there's no monorepo).

**Section A: Issue tracker.**

> Explainer: The "issue tracker" is where issues live for this repo. Skills like `to-tickets`, `triage`, and `to-spec` read from and write to it. They need to know whether to write a markdown file under `docs/tasks/`, call `gh issue create`, or follow some other workflow you describe. Pick the place you actually track work for this repo.

Default posture: the local markdown tracker at `docs/tasks/<feature>/` is the recommended default. Specs and tickets are committed to git, so work follows its branch and collaborators see it; propose it first. If the user prefers a hosted tracker, propose GitHub when a `git remote` points at GitHub, GitLab when it points at GitLab (`gitlab.com` or a self-hosted host). The options:

- **Local markdown**: issues live as files under `docs/tasks/<feature>/` in this repo (recommended default)
- **GitHub**: issues live in the repo's GitHub Issues (uses the `gh` CLI)
- **GitLab**: issues live in the repo's GitLab Issues (uses the [`glab`](https://gitlab.com/gitlab-org/cli) CLI)
- **Other** (Jira, Linear, etc.): ask the user to describe the workflow in one paragraph; the freeform template records it as prose

Record the choice in `docs/agents/issue-tracker.md`. The GitHub and GitLab templates carry a "PRs as a request surface" flag, defaulted **off**. Leave it off and don't raise it: a user who wants external PRs in the triage queue can flip the flag in the file later.

A GitHub or GitLab tracker needs a reachable repo. When the user picks one and no `git remote` points at that host, get the repo URL from them and wire it up (`git init` if needed, `git remote add origin <url>`) before going further. Done when `gh repo view` / `glab repo view` prints the project from inside this directory; if it can't, stop and tell the user what's missing.

**Section B: Triage label vocabulary.** Skip this section entirely if the `triage` skill isn't installed (exploration told you), since an uninstalled skill needs no labels.

If it is installed, ask exactly one question:

> Do you want to keep the default triage labels? (recommended: **yes**)

The defaults are the five canonical roles, each label string equal to its name: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. On **yes**, write them as-is. Only if the user says no, usually because their tracker already uses other names (e.g. `bug:triage` for `needs-triage`), collect the overrides so `triage` applies existing labels instead of creating duplicates.

**Section C: Domain docs.** Default to **single-context** (one `docs/glossary.md` + `docs/adr/`). This fits almost every repo; write it without asking. If the user asks for multi-context directly, honor that request.

Offer **multi-context** (a root `docs/glossary-map.md` pointing at per-context glossaries at `src/<context>/docs/glossary.md`) only when exploration found monorepo signals. Then confirm which layout they want.

Only the chosen variant is written: `docs/agents/domain.md` describes the layout this repo actually uses, never both.

### 3. Confirm and edit

Show the user a draft of:

- The `## Agent skills` block to add to whichever of `CLAUDE.md` / `AGENTS.md` is being edited (see step 4 for selection rules)
- The writing-standard block from [writing-standard.md](./writing-standard.md). The delimiters and version are part of the block's contract, so its wording is not edited here: wording changes ship only after an approved change list, via `update-engineering-skills`.
- The contents of `docs/agents/issue-tracker.md`, `docs/agents/domain.md`, and `docs/agents/triage-labels.md` (the last only when `triage` is installed)

Let them edit before writing.

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

### Issue tracker

[one-line summary of where issues are tracked]. See `docs/agents/issue-tracker.md`.

### Triage labels

[one-line summary of the label vocabulary]. See `docs/agents/triage-labels.md`.

### Domain docs

[one-line summary of layout: "single-context" or "multi-context"]. See `docs/agents/domain.md`.
```

Include the `### Triage labels` sub-block, and write `docs/agents/triage-labels.md`, only when `triage` is installed and Section B ran. When it isn't, both are omitted.

Then copy the writing-standard block from [writing-standard.md](./writing-standard.md) verbatim, delimiters included, into the steering file below the agent-skills block. If a delimited writing-standard block already exists there, replace its contents in place rather than appending a second block.

When Section B ran on GitHub or GitLab, create each configured label the tracker lacks (`gh label create` / `glab label create`).

Then write the docs files using the seed templates in this skill folder as a starting point:

- [issue-tracker-local.md](./issue-tracker-local.md): local-markdown issue tracker (the default)
- [issue-tracker-github.md](./issue-tracker-github.md): GitHub issue tracker
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md): GitLab issue tracker
- [issue-tracker-freeform.md](./issue-tracker-freeform.md): any other workflow, filled in from the user's description
- [triage-labels.md](./triage-labels.md): label mapping (only if `triage` is installed)
- [domain-single-context.md](./domain-single-context.md) or [domain-multi-context.md](./domain-multi-context.md), written to `docs/agents/domain.md`: only the chosen variant

### 5. Done

Tell the user the setup is complete and which engineering skills will now read from these files. Mention they can edit `docs/agents/*.md` directly later; re-running this skill is only necessary if they want to switch issue trackers or restart from scratch. After a skills update, `/update-engineering-skills` refreshes the skill files and migrates these managed artifacts, preserving everything custom.
