# Writing standard ships as a managed steering-file block

The response and writing standard lives as a versioned, delimited block inside the steering file (AGENTS.md, or CLAUDE.md where that is the existing file), not as a separate document reached by pointer. The standard must reach the agent on turn one of every session, and pointer-based delivery missed reliably enough that the standard was moved inline; a future reader who "fixes" the inline block back into a linked doc would reintroduce that observed failure.

## Consequences

The block is always-loaded context in every session. Wording changes ship only after an approved change list: exact diffs with reasoning and expected effect on model output. update-engineering-skills replaces the block in place, so hand edits to the block are overwritten; custom extensions belong in unmanaged files.
