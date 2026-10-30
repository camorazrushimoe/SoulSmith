# Skill Registry

This folder will hold the SoulSmith skill registry: the catalog of reusable skills the composer matches against when assembling an agent.

> **Status:** placeholder. No skills have been added yet.

## Manifest

Every skill will ship with a YAML manifest (as frontmatter in its file). The manifest is what the matcher reads, so it must be precise.

| Field | Required | Description |
|---|---|---|
| `id` | yes | Unique, stable, kebab-case identifier (e.g. `github-pr-diff-review`) |
| `name` | yes | Short human-readable name |
| `description` | yes | What the skill does, when to use it, and when not to. Written for a matcher, not a reader |
| `domain` | yes | Primary domain: `dev`, `qa`, `research`, `ops`, `delivery`, ... |
| `tags` | yes | Keywords that help retrieval |
| `tools.required` | yes | Tools the skill cannot work without |
| `tools.optional` | no | Tools that improve the result if present |
| `input_schema` | yes | What the skill expects as input |
| `output_schema` | yes | What the skill produces |
| `compatibility` | no | Runtimes, MCP servers, or platforms it is known to work with |
| `version` | yes | Semantic version of the skill |
| `author` | yes | Who wrote or maintains it |

### Example manifest (illustrative)

```yaml
---
id: test-gap-analysis
name: Test gap analysis
description: >
  Given a pull request diff, identifies changed functions and branches that
  have no corresponding test changes. Use for QA review of code PRs. Not for
  documentation-only or config-only changes.
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
author: soulsmith
---
```

## Index

_No skills yet._ Skills will be listed here by domain as they are added.

See [AGENT.md](AGENT.md) for how to add a skill.
