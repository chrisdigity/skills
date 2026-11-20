---
name: cleanup
description: Remove finished features' local tracker state - the feature directory with its spec, tickets, comments, and research - once all its tickets are resolved. Runs at the end of implement and implement-spec, or standalone to sweep the tracker.
---

Finished work leaves state behind: the spec that proposed it, the tickets that drove it, the comments and research that fed it. The durable record is the glossary and the ADRs written along the way; git history retains whatever the workspace drops. This skill removes the state.

The issue tracker config should have been provided to you (the repo's `docs/agents/issue-tracker.md` or the conversation). If not, use the **fallback root**: a stand-in repository root in the OS temp directory at `%TEMP%/<repo-slug>/` (`$TMPDIR`, else `/tmp`; `%TEMP%` on Windows; the slug is `<owner>-<repo>` from the git remote when one exists, else the checkout directory's name), with a local markdown tracker under it at `docs/tasks/`. Every `docs/` convention reads under the fallback root; write nothing into the real repo. State the fallback path to the user. The fallback root is disposable - the OS may clear it, so confirm it exists at session start; its state is outside git, so removal there is final, guarded by the containment check and the standalone confirmation. On a hosted tracker (GitHub, GitLab, ...) the tracker's state lives on the host and there is no local state to remove: report that and stop.

## What it removes

A feature's directory under the tracker path, wholesale: `spec.md`, `issues/`, `research/`, comments, everything in it. Wayfinder efforts too: a map directory whose child tickets are all resolved, its `research/` included.

## What it protects

Never removed: the glossary, the ADRs, and the rejected-request KB. These exist to persist and steer future agents. Anything a durable document needs from the state being removed must already be distilled into it; if it is not, surface that instead of deleting.

## When work is finished

A feature is finished when every ticket in its directory is resolved or closed the way the tracker config closes work. A wayfinder effort is finished when every child ticket on its map is resolved.

## Invocation

Two modes.

**Inline** (implement and implement-spec call it as their last step): the feature that just finished is known. Verify all its tickets are resolved, then remove it. No further confirmation: the work finished in this session.

**Standalone** (`/cleanup`): scan the tracker path for feature directories and effort maps whose tickets are all resolved, present the candidates oldest first, and wait for the user to pick before removing anything. Nothing is deleted without confirmation in this mode.

For every removal:

1. **Containment check.** Nothing outside the directory may reference anything inside it. Scan the glossary, the ADRs, the steering file, the other tracker directories, and any research outside the directory for references into it. If one is found, surface it and stop; the removal does not proceed until the reference is resolved.
2. **Remove the directory.** Git history retains it. On a project without git the deletion is final: the containment check and the standalone confirmation are the only guards.
