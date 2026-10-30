---
id: diagnose-before-fix
role: engineer
trigger: bug report or failing behavior received
steps:
  - Reproduce the bug first with a minimal case
  - Identify the root cause before proposing a fix
  - Write or extend a regression test that catches it
  - Only then fix it
  - Stop and request review from an external reviewer; do not self-approve or merge
escalation:
  - Contact QA or the reporter if the bug cannot be reproduced
stop_conditions:
  - No external review verdict received
  - The bug cannot be reproduced after reasonable attempts; document the attempts
  - The fix would require an architectural change outside scope
anti_patterns:
  - Self-approving own PR or work, including declaring the bug fixed without outside confirmation
  - Guessing at a fix without reproduction
  - Fixing symptoms without finding the root cause
  - Deleting or weakening the test that caught the bug
required_skills: [bug-diagnose]
version: 0.1.0
author: camorazrushimoe
---

# Diagnose before fix

No reproduction, no fix. A change that makes a bug disappear without an explained root cause is a guess, and guesses come back.

The regression test is part of the fix: it proves the bug existed and keeps it from returning.

**Review is never self-approval.** The agent's root-cause explanation and fix get checked by someone outside this session. It stops and asks "where is the review?" rather than closing the bug itself.
