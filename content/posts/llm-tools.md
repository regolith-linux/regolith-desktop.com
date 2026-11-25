---
authors:
  - name: Ken Gilmer
    link: https://github.com/kgilmer
    image: https://avatars.githubusercontent.com/u/49683
date: 2026-11-18
linktitle: LLM Usage in the Regolith Desktop project
title: LLM Usage in the Regolith Desktop project
---

Given the power, freshness, and controversy surrounding LLM-assisted software development, I think it's worth providing some details about how these tools are or are not being used in the project.

The Regolith Desktop 3.5 release includes many fixes and minor features driven from commercial LLM-based coding tools like Claude Code and Meta's Muse.  This release includes more fixes than any Regolith release in recent memory, perhaps ever.  These quality improvements are possible in part due to excellent test methods and infrastructure, and a systematic approach to root causing issues that these tools provide.  Due to this, I think Regolith 3.5 will be an excellent release, and will provide users with a robust operating environment for their computers.

I think the following guideliness for LLM-assisted contributions to the Regolith Desktop will ensure that quality of submissions is high and the time of people wishing to make improvements is respected.

For PR submissions involving LLM-based tools, we look for the following:
* A PR should be focused on a single specific bug or fine-grained feature.  Complex features should provide a strong general justification and be broken into a composable set of smaller PRs.
* A PR should address a bug or feature that is general to the Regolith user base.
* Run-time tests are important.  Meaning, tests that exercise the system in the same way a user would.  
* Test documentation in the PR should be provided to allow another LLM tool or person to reproduce the test if desired.
* The test work and reporting in a PR should be proportional to the change itself.  Meaning, a bug fix that has a large impact should have a correspondingly comprehensive set of tests and documentation in the PR.  A minor fix isolated to a single specific action or workflow needs correspondingly concise testing.
* PRs are submitted and owned by people, not tools. A PR submitter should have a reasonably strong understanding of what the PR code does, in particular the "why" of the change.
