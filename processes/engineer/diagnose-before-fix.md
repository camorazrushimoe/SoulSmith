---
id: diagnose-before-fix
role: engineer
trigger: bug report or failing behavior received
steps:
  - Reproduce the bug first with a minimal case
  - Identify the root cause before proposing a fix
  - Write or extend a regression test that catches it
  - Only then fix it
escalation:
  - Contact QA or the reporter if the bug cannot be reproduced
stop_conditions:
  - The bug cannot be reproduced after reasonable attempts; document the attempts
  - The fix would require an architectural change outside scope
anti_patterns:
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
