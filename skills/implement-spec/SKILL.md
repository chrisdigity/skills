---
name: implement-spec
description: "Implement the result of /to-spec and /to-tickets in code."
disable-model-invocation: true
---

You have been provided a spec. This spec should have tickets associated with it, describing how to implement the spec.

The issue tracker should have been provided to you. If not, tell the user to run `/setup-engineering-skills`.

The goal is the entire spec implemented on a single **integration branch**, with every ticket resolved the way the issue tracker closes work.

The tickets are not a list of steps. They are a **task graph** with blocking relationships between them. This means there is always a **frontier** of tickets which are ready to be grabbed.

Start every subagent with a fresh context window.

Communication to and from subagents should be sparse. Communicate primarily through **context pointers**: to the spec, tickets, research notes, and previous commits. Don't duplicate information already available via pointers.

**Implementer subagents** should be run in the background where possible for maximum concurrency.

## Steps

1. Read the spec and tickets to understand the task graph.

2. (optional) Use an **exploration subagent** to conduct any exploration required by the tickets - relevant codebase files or external documentation. Ensure the exploration subagent can save files - it should save its markdown notes in a directory outside the repo, accessible by all future subagents. This lets **implementer subagents** focus on implementation rather than exploration.

3. Create the integration branch. If the issue tracker closes work through PRs, or the user asks for one, open a draft PR after the first merge in step 5 (a branch with no commits ahead of main can't open one), marked as closing the spec and tickets.

4. Use a fresh **implementer subagent** for each ticket, each in its own worktree on its own branch. Each implementer subagent:
   - confirms its worktree is based on the integration branch before starting, and resets onto it if not;
   - calls the Skill tool with `tdd` to build the ticket;
   - merges the integration branch tip into its own branch before reporting done

5. Once an **implementer subagent** completes, merge its work to the integration branch with a **merger subagent**.

6. If this changes the **frontier** of available tickets, kick off more **implementer subagents** to work on the new tickets. This allows for maximum concurrency.

7. Once all tickets are complete, call the Skill tool with `code-review` on the integration branch. Fix all issues raised by the code review in a single fresh **implementer subagent**.

8. If a draft PR exists, mark it ready for review. Otherwise, resolve each ticket the way the issue tracker closes work, and report the integration branch.

9. Close out with promotion on the integration branch. The tracker holds what is proposed; the durable docs hold how the system should now behave, so the finished feature is distilled out of the tracker before its directory goes away:

   - Distill the behavior into the matching subsystem spec at `docs/specs/<subsystem>.md`; if none covers the subsystem, create one named with the kebab-case glossary term for it, with the four sections (Purpose, Behavior, Interfaces and contracts, Decisions in force) and no feature slugs or proposed-work marks.
   - Offer an ADR at `docs/adr/` for each hard-to-reverse decision the work made; write the ones the user confirms.
   - Add new terms to the glossary at the location `docs/agents/domain.md` describes, `docs/glossary.md` by default.
   - Confirm every tracker entry is resolved the way the issue tracker closes work.
   - Remove the feature directory last, once its essence lives in the durable docs; git history retains the rest.

   Commit the promotion edits. Promotion is done only when all five outcomes are checkable: specs updated, ADRs offered, glossary updated, tracker entries resolved, feature directory removed.

10. Clean up all **implementer subagent** worktrees.
