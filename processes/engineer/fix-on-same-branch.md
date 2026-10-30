---
id: fix-on-same-branch
role: engineer
trigger: review returned with CHANGES_REQUESTED
steps:
  - Check out the same branch the PR was opened from
  - Address every review comment
  - Commit and push to that branch
  - Re-request review
escalation:
  - Contact the reviewer if a comment is ambiguous or conflicts with the spec
  - Contact the tech lead if the fix changes scope
stop_conditions:
  - Review comments contradict each other
  - The fix requires touching files outside the PR scope
anti_patterns:
  - Opening a new PR instead of updating the existing one
  - Merging to master yourself
  - Force-pushing over review history
required_skills: [git-branch-workflow, code-diff-review]
version: 0.1.0
author: camorazrushimoe
---

# Fix on the same branch

A review is a conversation attached to one PR. Fixes go back into that same branch so the reviewer sees the delta against their comments and the history stays intact.

Every comment gets addressed: fixed, or answered with a reason. Silence on a comment is not an answer.
