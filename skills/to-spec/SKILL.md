---
name: to-spec
description: "Turn the current conversation into a spec and publish it to the project issue tracker: no interview, just synthesis of what you've already discussed."
disable-model-invocation: true
---

This skill takes the current conversation context and codebase understanding and produces a spec. Do NOT interview the user; just synthesize what you already know.

The issue tracker and triage label vocabulary should have been provided to you (the repo's `docs/agents/` files or the conversation). If not, use the **fallback root**: a stand-in repository root in the OS temp directory at `%TEMP%/<repo-slug>/` (`$TMPDIR`, else `/tmp`; `%TEMP%` on Windows; the slug is `<owner>-<repo>` from the git remote when one exists, else the checkout directory's name), with a local markdown tracker under it at `docs/tasks/` and the default triage label vocabulary. Every `docs/` convention reads under the fallback root; write nothing into the real repo. State the fallback path to the user. The fallback root is disposable - the OS may clear it, so confirm it exists at session start and never treat it as durable.

## Process

1. Explore the repo to understand the current state of the codebase, if you haven't already. Use the project's domain glossary vocabulary throughout the spec, and respect any ADRs in the area you're touching.

2. Sketch out the seams at which you're going to test the feature. Existing seams should be preferred to new ones. Use the highest seam possible. If new seams are needed, propose them at the highest point you can. The fewer seams across the codebase, the better - the ideal number is one.

Check with the user that these seams match their expectations.

3. Write the spec using the template below, then publish it to the tracker path the issue-tracker config defines, `docs/tasks/<feature-slug>/spec.md` by default. Apply the `ready-for-agent` triage label; no need for additional triage. Any research the feature consumed from `docs/tmp/` moves into the feature's `research/` directory at publish time (`git mv docs/tmp/<topic>.md docs/tasks/<feature-slug>/research/<topic>.md`), so the research is removed with the feature's state.

<spec-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A LONG, numbered list of user stories. Each user story should be in the format of:

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

This list of user stories should be extremely extensive and cover all aspects of the feature.

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified
- The interfaces of those modules that will be modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it within the relevant decision and note briefly that it came from a prototype. Trim to the decision-rich parts, not a working demo, just the important bits.

## Testing Decisions

A list of testing decisions that were made. Include:

- A description of what makes a good test (only test external behavior, not implementation details)
- Which modules will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope

A description of the things that are out of scope for this spec.

## Further Notes

Any further notes about the feature.

</spec-template>

4. Advise on tickets. Estimate how many vertical slices the spec implies: clusters of user stories that are each demoable end-to-end and buildable independently, each sized to fit a fresh context window. If the spec implies more than one, recommend running `/to-tickets` before any implementation. If it implies one, say the spec is ready for `/implement` as-is. Advice only: the user decides.
