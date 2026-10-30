---
id: spec-implement
name: Implement from spec or tickets
description: >
  Implement the work described in a spec or set of tickets on the current
  feature branch: test-first at pre-agreed seams, frequent typechecks and
  single-file test runs, full suite once at the end, self-review, then commit.
  Use when a spec or ticket is accepted and ready for code. Not for writing
  the spec (use spec-write), exploring design options (use prototype-build) or
  fixing an undiagnosed bug (use bug-diagnose).
domain: dev
tags: [implementation, coding, tdd, feature-branch, spec]
required_tools: [git, shell, file-edit]
optional_tools: [test-runner, typechecker]
input_schema:
  spec_or_tickets: string
  branch: string
output_schema:
  commits: list
  test_results: string
compatibility: [hermes-agent, pi]
version: 0.1.0
author: camorazrushimoe
---

# Implement from spec or tickets

Implement the work described by the user in the spec or tickets.

Use /tdd where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, use /code-review to review the work.

Commit your work to the current branch.

## Compact variant (forgeyard)

Require `spec.md`. Work only on `feature/<issue>-<slug>`.

Use TDD at seams. Run the smallest relevant tests often, full suite once at the end.
Self-review with code-review before you open or update the PR.
Commit in English, small logical commits. No `git add -A`.

## Anti-patterns

- Starting implementation without an accepted spec or ticket.
- Working on main instead of a feature branch.
- Running only the full test suite at the end instead of small tests during the work.
- Skipping the self-review step before committing or updating the PR.
- Using `git add -A` and committing unrelated changes.

---

Source: `dev-crew/agents/developer/skills/implement/SKILL.md`; `agent-office/agents/architect|staff-engineer/skills/implement/SKILL.md (same SHA)`; `forgeyard/skills/implement/SKILL.md (condensed rewrite, merged)`.
