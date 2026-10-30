# AGENT.md – Adding Skills to SoulSmith

This guide is for agents (and humans) adding skills to the registry. Follow it exactly. The registry is only as good as its manifests.

## 1. Write for the matcher, not for a human

Your `description` will be read by an embedder and a matcher that decide whether this skill fits a request. Write it so a machine can make that call correctly.

- State **what the skill does**, **when to use it**, and **when not to use it**.
- Name concrete inputs, outputs, artifacts, and platforms ("GitHub pull request diff", "pytest", "Postgres").
- Avoid marketing words: "powerful", "seamless", "smart", "comprehensive".
- Avoid vague verbs: "handles", "manages", "helps with".

**Specificity beats beauty.**

Bad:
> A powerful skill that helps with testing.

Good:
> Given a GitHub pull request diff, lists changed functions and branches with no matching test changes. Use for QA review of code PRs. Not for docs-only or config-only changes.

## 2. Required fields

Every manifest must include:

- `id`
- `name`
- `description`
- `domain`
- `tags`
- `tools.required`
- `input_schema`
- `output_schema`
- `version`
- `author`

Optional: `tools.optional`, `compatibility`.

See [README.md](README.md) for field definitions.

## 3. Naming conventions

- `id`: kebab-case, lowercase, stable once published. Pattern: `<object>-<action>` or `<platform>-<object>-<action>` (e.g. `test-gap-analysis`, `github-pr-diff-review`).
- File name equals the `id`: `skills/<domain>/<id>.md`.
- `domain`: one of `dev`, `qa`, `research`, `ops`, `delivery`. Propose new domains in an issue first.
- `tags`: lowercase, kebab-case, 3 to 8 tags.
- `version`: semantic versioning, start at `0.1.0`.

## 4. One skill per file

Each file describes exactly one skill. If a skill needs to do two unrelated things, split it into two skills. Composition is SoulSmith's job, not the skill's.

## 5. File pattern: YAML frontmatter + Markdown body

```markdown
---
id: test-gap-analysis
name: Test gap analysis
description: >
  Given a GitHub pull request diff, lists changed functions and branches
  with no matching test changes. Use for QA review of code PRs. Not for
  docs-only or config-only changes.
domain: qa
tags: [testing, coverage, pull-request, review]
tools:
  required: [github.get_pull_request_files]
  optional: [github.get_file_contents]
input_schema:
  pr_url: string
output_schema:
  untested_changes: list
compatibility: [github-mcp]
version: 0.1.0
author: your-handle
---

# Test gap analysis

## Steps
1. ...

## Output format
...

## Edge cases
...
```

The frontmatter is for matching. The body is the instructions the assembled agent follows.

## 6. Before you commit

- [ ] `id` is unique in the registry
- [ ] All required fields are present
- [ ] Description says when *not* to use the skill
- [ ] Required tools actually exist in the named connectors
- [ ] Skill is added to the index in `skills/README.md`
