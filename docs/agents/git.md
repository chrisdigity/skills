# Git

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
2. Report the failure to the maintainer before diagnosing further: the command that failed, the error text, and that the requirement likely sits on their machine. Ask permission before searching for the cause; do not assume the remedy.
3. With permission, investigate and offer known remedies instead of performing them. For signing, the usual one is the maintainer's passphrase or PIN entry ritual; its common form, run by the maintainer in their own terminal:

   ```
   echo | gpg --clear-sign
   ```

   prompts for the key's passphrase or PIN and caches it, unblocking signed commits for a period of time. A failed push may instead need the credential helper refreshed by re-authenticating in the maintainer's terminal or browser.
4. Retry the operation unchanged once the maintainer confirms the requirement is satisfied.
