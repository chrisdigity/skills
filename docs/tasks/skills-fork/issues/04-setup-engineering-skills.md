# 04: setup-engineering-skills

What to build: The new setup skill with its seed templates, following upstream's prompt-driven process (explore, present, confirm, write). Seeds: issue-tracker-local.md (conventions for `docs/tasks/<feature>/`, including wayfinding operations), issue-tracker-github.md and issue-tracker-gitlab.md carried from upstream unchanged, a freeform option, domain.md in both variants (single-context and multi-context; only the chosen variant is emitted into a project's `docs/agents/domain.md`), triage-labels.md, and the writing-standard seed block (the currently approved text, wrapped in versioned delimiters). Setup writes the agent-skills block into the steering file (AGENTS.md, or CLAUDE.md where that is what exists).

Blocked by: 03

Status: ready-for-agent

- [ ] `skills/setup-engineering-skills/` exists with SKILL.md and all seed templates.
- [ ] A scratch-repo run produces `docs/agents/issue-tracker.md`, `docs/agents/domain.md` (chosen variant only), `docs/agents/triage-labels.md`, and the writing-standard block in the steering file.
- [ ] The local tracker template points at `docs/tasks/<feature>/` with the settled file shape (spec.md, issues/NN-slug.md, Status lines, wayfinding operations).
- [ ] domain.md carries the precedence rule and the check-the-tracker consumer step, as plain prose.
- [ ] The writing-standard block carries versioned delimiters so update can replace it in place.
- [ ] Import log entry records the supersession.
