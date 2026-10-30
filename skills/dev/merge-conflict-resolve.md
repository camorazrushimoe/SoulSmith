---
id: merge-conflict-resolve
name: Merge conflict resolution
description: >
  Resolve an in-progress git merge or rebase conflict: inspect state, read the
  commits, PRs and tickets behind each side, resolve each hunk preserving both
  intents, run typecheck/tests/format, then finish the merge or rebase. Use
  when git reports conflicts on a feature branch. Not for choosing a branching
  strategy (use git-branch-workflow) or for general code review.
domain: dev
tags: [git, merge-conflicts, rebase, feature-branch]
required_tools: [git, shell]
optional_tools: [github, issue-tracker]
input_schema:
  repo_path: string
  operation: "enum(merge|rebase)"
output_schema:
  resolved_commit: string
  checks_result: string
compatibility: [git, hermes-agent, pi]
version: 0.1.0
author: camorazrushimoe
---

# Merge conflict resolution

1. **See the current state** of the merge/rebase. Check git history, and the conflicting files.

2. **Find the primary sources** for each conflict. Understand deeply why each change was made, and what the original intent was. Read the commit messages, check the PRs, check original issues/tickets.

3. **Resolve each hunk.** Preserve both intents where possible. Where incompatible, pick the one matching the merge's stated goal and note the trade-off. Do **not** invent new behaviour. Always resolve; never `--abort`.

4. Discover the project's **automated checks** and run them — typically typecheck, then tests, then format. Fix anything the merge broke.

5. **Finish the merge/rebase.** Stage everything and commit. If rebasing, continue the rebase process until all commits are rebased.

## Compact variant (forgeyard)

Merge or rebase main onto the feature branch only.
Keep both sides' tests green.
Do not resolve by deleting the other side's behaviour unless the spec says so.
Never force-push to main.

## Anti-patterns

- Resolving by deleting the other side's behaviour without the spec saying so.
- Inventing new behaviour while resolving a hunk.
- Aborting the merge or rebase instead of resolving it.
- Force-pushing to main.
- Committing the resolution without running the project's checks.

---

Source: `dev-crew/agents/developer/skills/resolving-merge-conflicts/SKILL.md`; `agent-office/agents/architect|staff-engineer/skills/resolving-merge-conflicts/SKILL.md (same SHA)`; `forgeyard/skills/resolving-merge-conflicts/SKILL.md (condensed rewrite, merged)`.
