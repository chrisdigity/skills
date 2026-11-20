# Git operations

What git operations agents may perform in this repo, and what to do when a git operation fails on a requirement only the maintainer can satisfy.

## What agents may do

- Commit to the current branch: yes, once the working tree state is understood.
- Push: agents never push on their own; the maintainer pushes, or explicitly asks for it.
- Force push: never, unless the maintainer explicitly asks in the moment.
- Rewriting shared history (rebase, reset, or filter operations on branches others build on): ask first.

The Push line is set during setup from the user's answer; the default recorded here is no. Edit the list to match how the repo actually works.

## Commit requirements

Commit requirements live with each maintainer's machine, not in the repository: one maintainer signs commits while another does not, a hardware signing key caches its PIN per machine, and a credential helper logs in per account. The same commit can therefore succeed for one maintainer and fail for another, and the difference is invisible from inside the repository. Requirements the repository itself enforces for everyone, such as branch protection demanding signed commits or CI checking a sign-off line, can be recorded in this section by the maintainer.

## When a git operation fails

A failed commit or push is not always a repo defect: as described above, the failing requirement usually sits outside the repository, on the maintainer's machine. Only the maintainer can satisfy it, so diagnosing inside the repository cannot fix it.

When a commit or push fails with an error about signing, passphrases, agents, credentials, or hooks:

1. Read the error, then stop. Do not disable the requirement (no `--no-gpg-sign`, no `--no-verify`), do not edit signing or pinentry configuration, and do not restart gpg or credential agents: a restart flushes whatever the maintainer has cached.
2. Classify the failure from the error text before reporting anything. Two classes carry a remedy; everything else is unclassified:
   - **Locked key**: the key is waiting to be unlocked. Signs: a prompt or a hang, a passphrase or PIN request, `bad passphrase`, `Inappropriate ioctl for device`. Remedy: the maintainer's passphrase or PIN entry ritual (step 4). A failure that never asks for anything - gpg erroring out immediately, a missing tool - is never a locked key.
   - **Credentials** (pushes): the remote rejected the authentication. Signs: `Authentication failed`, `could not read Username`, HTTP 401 from the remote. Remedy: the maintainer re-authenticates in their own terminal or browser.
   - **Unclassified**: everything else, whatever the suspicion the error text raises. Report it and wait: the maintainer diagnoses the cause. The agent offers no remedy and asserts no cause, however plausible it looks - a plausible cause read off an error message is a guess about a machine the agent cannot see.
3. Report before diagnosing further: the command that failed, the exact error text, and, for a classified failure, the class with the evidence for it. Ask permission before investigating the cause; never offer a remedy the evidence does not support.
4. With permission, offer the remedy for a classified failure; perform nothing on the maintainer's machine yourself. For a locked key, the ritual - run by the maintainer in their own terminal:

   ```
   echo | gpg --clear-sign
   ```

   prompts for the key's passphrase or PIN and caches it, unblocking signed commits for a period of time. For a credential failure, the maintainer re-authenticates in their own terminal or browser.
5. Retry the operation unchanged once the maintainer confirms the requirement is satisfied. If the maintainer's diagnosis contradicts the classification, reclassify, report again, and wait: a misread class is a diagnosis to correct, not an instruction to push through.
