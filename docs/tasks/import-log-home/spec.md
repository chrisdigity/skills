# Import log home

Status: needs-triage

Problem: the README import log reads as a changelog and grows without bound. Every skill edit appends an entry, so the README carries an ever-longer record that most readers never need. The maintainer raised this on 2026-11-13 while confirming the ADR offers for the skills-fork promotion: "I also don't like the import log in the readme. Feels like a changelog, which gets very large over time and putting that in the readme is unnecessary bloat to the readme."

Request: move the import log out of the README to its own file, or otherwise bound its presence in the README. The log's job, recording every deliberate divergence so imports stay deliberate, is unchanged; only its home is in question.

Notes: captured directly from the maintainer, not yet through to-spec. The settled convention records the log in the README, so the skills-fork promotion intentionally left it where it is; this request proposes changing that and is the place where the change gets designed.

## Comments

Captured 2026-11-13 during ticket 07's promotion close-out, so the feedback survives the feature directory's removal.
