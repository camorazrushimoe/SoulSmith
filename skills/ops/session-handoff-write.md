---
id: session-handoff-write
name: Session handoff document
description: >
  Compact the current conversation into a handoff document a fresh agent
  session can continue from, saved to the OS temp directory, with suggested
  skills, references to existing artifacts instead of copies, and secrets
  redacted. Use when ending a session whose work will continue elsewhere. Not
  for packaging research findings for a product team (use
  research-handoff-package).
domain: ops
tags: [handoff, context-transfer, session, summary]
required_tools: [file-write]
optional_tools: []
input_schema:
  next_session_focus: string
output_schema:
  handoff_doc_path: string
compatibility: [hermes-agent, claude-code]
version: 0.1.0
author: camorazrushimoe
---

# Session handoff document

Write a handoff document summarising the current conversation so a fresh agent can continue the work. Save to the temporary directory of the user's OS - not the current workspace.

Include a "suggested skills" section in the document, which suggests skills that the agent should invoke.

Do not duplicate content already captured in other artifacts (specs, plans, ADRs, issues, commits, diffs). Reference them by path or URL instead.

Redact any sensitive information, such as API keys, passwords, or personally identifiable information.

If the user passed arguments, treat them as a description of what the next session will focus on and tailor the doc accordingly.

## Anti-patterns

- Copying specs, plans, diffs or issues into the handoff instead of linking them.
- Leaving API keys, passwords or personal data in the document.
- Saving the handoff inside the workspace or repo.
- Omitting the suggested-skills section.

---

Source: `dev-crew/agents/tech-pm/skills/handoff/SKILL.md`.
