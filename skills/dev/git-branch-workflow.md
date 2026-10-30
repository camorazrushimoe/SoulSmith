---
id: git-branch-workflow
name: Git feature-branch workflow
description: >
  Rules for working in git on a feature branch: branch naming
  feature/<ticket>-<slug>, small English commits, push and open a pull
  request, never commit or push to main/master, and a list of blocked
  destructive commands. Use whenever an agent creates a branch, commits, or
  opens a PR. Not for resolving merge conflicts (use merge-conflict-resolve)
  or for reviewing code.
domain: dev
tags: [git, branching, pull-request, commits, guardrails]
required_tools: [git]
optional_tools: [github]
input_schema:
  ticket_id: string
  ticket_title: string
output_schema:
  branch_name: string
  pull_request_url: string
compatibility: [git, github, hermes-agent, pi]
version: 0.1.0
author: camorazrushimoe
---

# Git feature-branch workflow

The Dev Crew works on feature branches. **Never commit or push directly to `main`/`master`.**

## Branch naming

`feature/<ticket>-<slug>`, e.g. `feature/BON-15-login`. Lowercase slug, hyphen-separated, derived from the ticket title.

## Rules

1. Check out a new branch from `main` for every task: `git checkout -b feature/<ticket>-<slug>`.
2. Commit small, logical changes with clear messages in English.
3. Push the branch and open a pull request — do not merge directly.
4. Request review (the qa agent, or self-review via the `code-review` skill) before merging.
5. Delete the branch after merge.

## Blocked (destructive) commands

Never run: `git push` to `main`/`master`, `git reset --hard`, `git clean -fd`, `git branch -D` on an in-flight feature branch, `git checkout .` / `git restore .`.

## Compact variant (forgeyard)

Same rules; adds `git add -A` to the forbidden list and says not to merge your own PR.

Never commit or push to `main`/`master`.

Branch: `feature/<issue>-<slug>` from main.
Commit small, English messages.
Push the branch and open a PR. Do not merge it.

Forbidden: `git push` to main, `git reset --hard`, `git clean -fd`, deleting an in-flight feature branch, `git add -A`.

## Anti-patterns

- Committing or pushing directly to main/master, even for a one-line fix.
- Using `git add -A` and committing unrelated or generated files.
- Running `git reset --hard`, `git clean -fd` or `git checkout .` on a branch with in-flight work.
- Merging your own PR without review.
- Large mixed-purpose commits with vague messages.

---

Source: `dev-crew/agents/developer|qa/skills/git-branch-discipline/SKILL.md (same SHA)`; `agent-office/agents/architect|staff-engineer/skills/git-branch-discipline/SKILL.md (same SHA)`; `forgeyard/skills/git-branch-discipline/SKILL.md (condensed rewrite, merged)`.
