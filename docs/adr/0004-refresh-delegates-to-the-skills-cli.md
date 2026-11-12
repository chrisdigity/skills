# Skill-file refresh delegates to the skills CLI

update-engineering-skills implements no installer of its own: file refresh delegates to the skills CLI (check, then update), which installs from the same source the fork publishes and verifies with the content hashes in skills-lock.json. A parallel installer would fork the install state: the lockfile verifies only what the CLI wrote, so files a custom installer wrote would be invisible to that verification, and local-change detection could disagree between the two paths. The skill's own instructions are spent entirely on migrating the managed artifacts.

## Consequences

The fork tracks the CLI's behavior (exit codes, scopes, flags) instead of reimplementing it, so CLI changes are adopted by importing them here, not by editing the skill. Refresh reads the seed templates from the installed setup-engineering-skills folder the CLI refreshed in the same run; a missing sibling folder stops the run with a reinstall pointer rather than regenerating from memory, so the pair ships together.
