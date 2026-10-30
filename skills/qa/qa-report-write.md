---
id: qa-report-write
name: QA report
description: >
  Write a structured QA report after testing a change: scope, PASS/FAIL/NEEDS
  CHANGES result, tests run, bugs with severity and reproduction steps,
  coverage gaps and a merge verdict; post it to the ticket and the bus. Use
  when reporting test results, bug findings or a merge verdict on a PR. Not
  for diagnosing a bug's root cause (use bug-diagnose) or reviewing code style
  (use code-diff-review).
domain: qa
tags: [qa, test-report, bug-report, merge-verdict, severity]
required_tools: [test-runner]
optional_tools: [linear, github, redis-bus]
input_schema:
  ticket: string
  pr_or_branch: string
output_schema:
  result: "enum(PASS|FAIL|NEEDS CHANGES)"
  bugs: list
  verdict: string
compatibility: [hermes-agent, linear, github]
version: 0.1.0
author: camorazrushimoe
---

# QA report

After verifying a change, produce a report with this structure (in English):

```
## QA Report — <ticket>

**Scope**: what was tested (ticket + brief).
**Result**: PASS / FAIL / NEEDS CHANGES.
**Tests run**: list of test commands + outcomes.
**Bugs found**:
- [severity] description + steps to reproduce + expected vs actual.
**Coverage gaps**: areas not tested and why.
**Verdict**: ready to merge / needs changes (list required fixes).
```

## Rules

- Severity: `blocker` (must fix), `major`, `minor`, `nits`.
- Every bug gets reproducible steps.
- Post the report to Linear (comment on the ticket) and to the bus.
- Be specific and factual — no vague "looks fine".

## Compact variant (forgeyard)

GitHub variant: post the report on the PR instead of Linear.

```
## QA Report

**Scope**: ...
**Result**: PASS | FAIL | NEEDS CHANGES
**Tests run**: command + outcome
**Bugs found**:
- [blocker|major|minor|nits] description; steps; expected vs actual
**Coverage gaps**: ...
**Verdict**: ready to merge | needs changes
```

Every bug has steps. Post on the PR or issue. No "looks fine".

## Anti-patterns

- "Looks fine" verdicts with no list of tests run.
- Bugs reported without reproduction steps or expected vs actual behaviour.
- Hiding coverage gaps instead of listing what was not tested and why.
- Merging the PR yourself after a PASS.

---

Source: `dev-crew/agents/qa/skills/qa-report/SKILL.md`; `forgeyard/skills/qa-report/SKILL.md (condensed rewrite, merged)`.
