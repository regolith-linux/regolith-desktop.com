---
authors:
  - name: Ken Gilmer
    link: https://github.com/kgilmer
    image: https://avatars.githubusercontent.com/u/49683
date: 2026-11-18
linktitle: LLM Usage in the Regolith Desktop project
title: LLM Usage in the Regolith Desktop project
---

Given the power and current controversy surrounding LLM-assisted software development, I think it's worth providing some details about how these tools are or are not being used in the project.

The Regolith Desktop 3.5 release includes many fixes and minor features driven from commercial LLM-based coding tools like Claude Code and Meta's Muse.  This release includes more fixes than any Regolith release in recent memory, perhaps ever.  These quality improvements are possible in part due to excellent test methods and infrastructure, and a systematic approach to root causing issues that these tools provide.  Due to this, I think Regolith 3.5 will be an excellent release, and will provide users with a robust operating environment for their computers.

For much of 2026, under the direction of my previous employer, I had been using LLM tools for the purposes of professional software development.  I've used LLM tooling across the spectrum of <i>"check my code, add tests, let me know if any errors are found"</i> to <i>"port this large complex system to this other platform I know nothing about, and make no mistakes"</i>.  The following constraints on submissions to the Regolith Desktop project are based on my learnings from this experience.

For PR submissions involving LLM-based tools, we look for the following:
* A PR should be focused on a single specific bug or fine-grained feature.  Complex features should provide a strong general justification and be broken into a composable set of smaller PRs.
* A PR should address a bug or feature that is general to the Regolith user base.
* Run-time tests are important.  Meaning, tests that exercise the system in the same way a user would.  
* Test documentation in the PR should be provided to allow another LLM tool or person to reproduce the test if desired.
* The test work and reporting in a PR should be proportional to the change itself.  Meaning, a bug fix that has a large impact should have a correspondingly comprehensive set of tests and documentation in the PR.  A minor fix isolated to a single specific action or workflow needs correspondingly concise testing.
* PRs are submitted and owned by people, not tools. A PR submitter should have a reasonably strong understanding of what the PR code does, in particular the "why" of the change.
