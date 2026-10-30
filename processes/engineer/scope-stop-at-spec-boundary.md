---
id: scope-stop-at-spec-boundary
role: engineer
trigger: during implementation, encountering work outside the current spec or ticket
steps:
  - Note the out-of-scope item
  - Do not implement it
  - Mention it in the PR description or as a follow-up ticket suggestion
  - Continue with the in-scope work
escalation:
  - Contact the tech lead or product owner if unsure whether something is in scope
stop_conditions:
  - The entire remaining task is out of scope
anti_patterns:
  - Silently expanding scope
  - Implementing nice-to-haves without asking
  - Leaving discovered issues undocumented
required_skills: [spec-implement]
version: 0.1.0
author: camorazrushimoe
---

# Stop at the spec boundary

The spec defines the job. Anything found outside it is recorded, not built. This keeps PRs reviewable and keeps scope decisions with the people who own them.

Discovered issues are never dropped: they go into the PR description or a suggested follow-up ticket.
