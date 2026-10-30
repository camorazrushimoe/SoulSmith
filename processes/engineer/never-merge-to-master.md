---
id: never-merge-to-master
role: engineer
trigger: agent has completed work on a branch and is ready to integrate
steps:
  - Create or check out a feature branch from the latest main
  - Make commits on that branch only
  - Open a pull request
  - Wait for the review verdict
escalation:
  - Contact the tech lead if review is blocked for more than 24h or the reviewer is unresponsive
stop_conditions:
  - The PR targets master/main directly from an unreviewed path
  - No open PR exists before declaring the work done
anti_patterns:
  - Merging to master/main yourself
  - Force-pushing to main
  - Pushing directly to main
required_skills: [git-branch-workflow]
version: 0.1.0
author: camorazrushimoe
---

# Never merge to master

Integration is a human decision. The engineer agent's job ends at an open, reviewable pull request. It never lands its own work on the main line.

Work is not "done" until a PR exists and has been handed to review. A green branch with no PR is unfinished work.
