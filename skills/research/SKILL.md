---
name: research
description: Investigate a question against high-trust primary sources and capture the findings as a Markdown file. Use when the user wants a topic researched, docs or API facts gathered, or reading legwork delegated to a background agent.
---

Spin up a **background agent** to do the research, so you keep working while it reads. Start it with a fresh context window.

Its job:

1. Investigate the question against **primary sources** (official docs, source code, specs, first-party APIs), not a secondary write-up of them. Follow every claim back to the source that owns it.
2. Write the findings to a single Markdown file, citing each claim's source, and date it.
3. Save it where its consumer lives: research serving a feature or effort in the tracker goes in that directory's `research/` subdirectory as `<topic>.md`; research tied to nothing yet goes to gitignored `docs/tmp/<topic>.md`, disposable like the rest of `docs/tmp/`. When the repo has no agent-docs configuration, every `docs/` location resolves under the **fallback root**: a stand-in repository root in the OS temp directory at `%TEMP%/<repo-slug>/` (`$TMPDIR`, else `/tmp`; `%TEMP%` on Windows; the slug is `<owner>-<repo>` from the git remote when one exists, else the checkout directory's name) - write nothing into the real repo. Say where you saved it, fallback path included: the findings are point-in-time, so a reader knows to re-verify them before relying on them.
4. Anything a durable document (the glossary, an ADR) needs from the findings is distilled into that document; durable documents never reference research by path. The file itself is removed by `/cleanup` with the feature it served, or by the usual fate of `docs/tmp/` if nothing ever adopts it.
