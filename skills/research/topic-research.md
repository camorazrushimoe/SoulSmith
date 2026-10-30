---
id: topic-research
name: Primary-source topic research
description: >
  Investigate a question against high-trust primary sources (official docs,
  specs, source code) and capture the findings with citations as a Markdown
  file in the repo. Use when a topic, API or documentation fact needs
  researching, or reading legwork can be delegated to a background agent. Not
  for running experiments (use experiment-design) or opinion surveys of blog
  posts.
domain: research
tags: [research, primary-sources, documentation, citations]
required_tools: [web-fetch, file-write]
optional_tools: [web-search, subagents]
input_schema:
  question: string
output_schema:
  findings_path: string
  sources: list
compatibility: [hermes-agent, claude-code]
version: 0.1.0
author: camorazrushimoe
---

# Primary-source topic research

Spin up a **background agent** to do the research, so you keep working while it reads.

Its job:

1. Investigate the question against **primary sources** — official docs, source code, specs, first-party APIs — not a secondary write-up of them. Follow every claim back to the source that owns it.
2. Write the findings to a single Markdown file, citing each claim's source.
3. Save it where the repo already keeps such notes; match the existing convention, and if there is none, put it somewhere sensible and say where.

## Anti-patterns

- Citing secondary blog posts when a primary source exists.
- Findings without links to the sources they came from.
- Answering from memory without opening the source.
- Leaving the findings in chat instead of a file in the repo.

---

Source: `dev-crew/agents/developer|qa/skills/research/SKILL.md (same SHA)`; `agent-office/agents/architect|staff-engineer/skills/research/SKILL.md (same SHA)`.
