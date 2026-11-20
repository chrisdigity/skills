# Unconfigured repos use a fallback root in the OS temp directory

The carried skills assumed a configured repo: without one, to-spec, to-tickets, triage, implement-spec, code-review, cleanup, wayfinder, and research dead-ended by telling the user to run /setup-agent-docs. But these skills are installed into repos the agent does not own, where the maintainer does not want the agent documentation committed, so there is no configuration to run and no legitimate place inside the repo for specs, tickets, research, and the rejected-request KB. The decision: when no tracker configuration has been provided (neither the repo's docs/agents/ files nor the conversation), skills resolve every docs/ location against a fallback root - a stand-in repository root in the OS temp directory at %TEMP%/<repo-slug>/ ($TMPDIR, else /tmp; %TEMP% on Windows; the slug is <owner>-<repo> from the git remote when one exists, else the checkout directory's name). All conventions hold verbatim beneath it (docs/tasks/, docs/tmp/, docs/out-of-scope/); nothing is written into the real repo; the skill states the path to the user. Domain docs are the carve-out: the glossary and ADRs are never created under the fallback root, and on repos the agent cannot mark, /grill-me is the interview - grill-with-docs remains for repos that welcome docs.

## Considered options

- A gitignored working space inside the unconfigured repo: rejected as more complex and more error-prone - it edits the maintainer's .gitignore, litters their status output with untracked files, and one missed entry leaks working files into a contributed PR.
- An external-tracker variant in setup-agent-docs: rejected - setup only writes into the repo it runs in, and a hosted-tracker choice marks the maintainer's own issue tracker; no in-repo config file can exist without contradicting the constraint it serves.
- A durable user-level directory (for example ~/.agent-docs/): rejected - the state is disposable by design; the OS temp directory matches the existing convention (handoff already writes there), and a durable location would encourage treating cross-repo state as permanent.

## Consequences

- The flow runs end to end on foreign repos - grill-me, to-spec, to-tickets, implement or implement-spec, code-review, cleanup - with every structured artifact outside the repo; the maintainer sees only the code diff.
- The fallback root is disposable: the OS may clear it, so skills confirm it exists at session start and never treat it as durable. It is outside git, so cleanup's removal of a finished feature directory there is final, guarded by the containment check and the standalone confirmation its without-git rule already provides.
- Two checkouts of the same repo (a main checkout and worktrees) share the slug and therefore the same fallback state; accepted as correct for one project.
- The configured-repo path is unchanged: skills read the repo's docs/agents/ configuration exactly as before, and setup-agent-docs is untouched - it offers no migration of fallback state, since temp state is disposable.
- to-spec additionally gained an advisory step (estimate the vertical slices the spec implies; recommend /to-tickets when there is more than one), recorded in the divergence ledger rather than here: it is advisory, easily reversible, and not surprising once read.
