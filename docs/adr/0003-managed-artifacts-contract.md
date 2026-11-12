# Setup and update own the managed artifacts

The per-topic configuration files under docs/agents/ and the writing-standard block in the steering file are owned by setup-engineering-skills and update-engineering-skills; everything else under docs/ is unmanaged and preserved as-is. The ownership line has to be unambiguous because update regenerates owned files byte-identical from the seed templates and replaces the block in place: a file with unclear ownership is either silently overwritten, losing the user's work, or silently skipped, drifting from the seed it is supposed to match. Custom extensions therefore belong in unmanaged files.

## Consequences

Hand edits to managed paths do not survive the next migration, so the docs/agents/ files are configuration, not freeform notes. A custom file found at a managed path is raised as a conflict instead of overwritten. The freeform tracker file is carved out: its workflow text is the user's own words, so it is never regenerated. Update enumerates all of docs/agents/ and classifies each file by seed title and shape rather than checking a fixed list of known paths, so a newly managed artifact is detected by the same rule as the existing ones.
