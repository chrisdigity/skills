# Issue tracker: local markdown

Issues and specs for this repo live as markdown files in `docs/tasks/`, committed to git.

## Conventions

- One feature per directory: `docs/tasks/<feature-slug>/`
- The spec is `docs/tasks/<feature-slug>/spec.md`
- Implementation issues are one file per ticket at `docs/tasks/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01`, never a single combined tickets file
- Triage state is recorded as a `Status:` line near the top of each issue file (see `triage-labels.md` for the role strings)
- Comments and conversation history append to the bottom of the file under a `## Comments` heading

## When a skill says "publish to the issue tracker"

Create a new file under `docs/tasks/<feature-slug>/` (creating the directory if needed).

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. The user will normally pass the path or the issue number directly.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a file with one **child** file per ticket.

- **Map**: `docs/tasks/<effort>/map.md` (the Notes / Decisions-so-far / Fog body).
- **Child ticket**: `docs/tasks/<effort>/issues/NN-<slug>.md`, numbered from `01`, with the question in the body. A `Type:` line records the ticket type (`research`/`prototype`/`grilling`/`task`); a `Status:` line records `claimed`/`resolved`.
- **Blocking**: a `Blocked by:` line near the top lists the tickets that gate this one; a blocker in another feature directory is referenced by its path. A ticket is unblocked when every ticket it lists is `resolved`.
- **Frontier**: scan `docs/tasks/<effort>/issues/` for files that are open, unblocked, and unclaimed; first by number wins.
- **Claim**: set `Status: claimed` and save before any work.
- **Resolve**: append the answer under an `## Answer` heading, set `Status: resolved`, then append a context pointer (gist + link) to the map's Decisions-so-far in `map.md`.
