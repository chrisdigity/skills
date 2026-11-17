# Installing internal skills

Skills marked `metadata.internal: true` are hidden from the skills CLI: `add` skips them, and `list` and `check` do not see them. Setting `INSTALL_INTERNAL_SKILLS=1` (or `true`) makes them visible and installable; the CLI documents this variable in its troubleshooting notes.

Install this repository's skills, including this one, into a project or the test install (from the repo root):

    INSTALL_INTERNAL_SKILLS=1 npx skills add chrisdigity/skills -y

Refresh an existing install:

    INSTALL_INTERNAL_SKILLS=1 npx skills update -y

Keep the variable set for `list` and `check` runs as well while the internal skill should stay visible: without it, `check` reports the installed internal skill as deleted upstream, and an interactive run will offer to delete it.
