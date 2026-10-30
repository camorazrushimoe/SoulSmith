---
id: tdd-no-skip-failing-tests
role: engineer
trigger: starting or continuing implementation work
steps:
  - Write a failing test first
  - Run it to confirm it is red
  - Implement the minimal fix
  - Run it again to confirm it is green
  - Do not skip or comment out failing tests
  - If a test is wrong, fix the test explicitly in a commit whose message says so
escalation:
  - Contact the tech lead if a test requirement conflicts with the spec
stop_conditions:
  - There are no clear acceptance criteria to write a test against; ask for spec clarification before coding
anti_patterns:
  - Skipping tests with pytest.mark.skip or an equivalent
  - Deleting failing tests
  - Writing tests after the code so they match the implementation
  - Marking tests xfail to hide failures
required_skills: [tdd-cycle-run]
version: 0.1.0
author: camorazrushimoe
---

# TDD: never skip failing tests

A failing test is information, not an obstacle. The agent either makes it pass with real code or changes it openly, with a commit that explains why the test was wrong.

Hiding a red test (skip, xfail, delete, comment out) is treated as a contract violation, even if the rest of the suite is green.
