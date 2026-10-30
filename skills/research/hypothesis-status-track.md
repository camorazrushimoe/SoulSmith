---
id: hypothesis-status-track
name: Hypothesis status tracking
description: >
  Keep the living status of hypotheses and experiments accurate and easy to
  scan in one canonical tracking file. Use whenever a hypothesis or experiment
  changes state. Not for writing the final report (use
  research-report-synthesize).
domain: research
tags: [tracking, hypotheses, status, research]
required_tools: [file-edit]
optional_tools: []
input_schema:
  hypothesis_id: string
  new_status: string
output_schema:
  tracking_file: markdown
compatibility: [lab-crew, hermes-agent]
version: 0.1.0
author: Sergey Ryabushko
---

# Hypothesis status tracking

## Purpose

Keep the living status of hypotheses and experiments accurate and easy to scan.

## When to use

- A hypothesis is created, started, supported, rejected, or parked
- An experiment starts or finishes
- Confidence or caveats change

## Canonical artifact

`workspace/<topic>/hypotheses.md` is the source of truth.

### Suggested format

```markdown
# Hypotheses — <topic>

| ID | Statement | Status | Confidence | Linked experiments | Notes |
|----|-----------|--------|------------|--------------------|-------|
| H1 | ...       | in-progress | medium | exp-price-drop | ... |
```

Status values: `proposed` | `in-progress` | `supported` | `rejected` | `inconclusive` | `parked`

## Discipline

- Update status as soon as evidence changes the picture
- Never leave a finished experiment without updating the linked hypothesis
- Keep notes short; details belong in the Research Report

## Anti-patterns

- Status scattered across chats instead of the canonical file.
- Statuses that say "in progress" long after work stopped.
- Marking a hypothesis confirmed without linking the evidence.
- Deleting refuted hypotheses instead of recording them as refuted.

---

Source: `lab-crew/skills/research-tracking/SKILL.md`.
