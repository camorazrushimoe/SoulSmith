---
id: research-report-synthesize
name: Research report synthesis
description: >
  Turn experiment results and evaluation outcomes into a Research Report with
  a fixed minimum structure: brief, hypotheses tested, what was done, key
  findings, limitations and risks, recommendations. Use when a research effort
  reaches a conclusion. Not for a single experiment's evaluation section (use
  eval-report-write) or the product handoff (use research-handoff-package).
domain: research
tags: [research-report, synthesis, findings, recommendations]
required_tools: [file-write]
optional_tools: []
input_schema:
  results: list
  hypotheses: list
output_schema:
  research_report: markdown
compatibility: [lab-crew, hermes-agent]
version: 0.1.0
author: Sergey Ryabushko
---

# Research report synthesis

## Purpose

Turn experiment results + evaluation outcomes into a clear Research Report.

## When to use

- One or more experiments have finished
- Evaluation Specialist has reported metrics / grading outcomes
- It is time to decide: supported / rejected / inconclusive / needs more research

## Research Report structure (minimum)

```markdown
# Research Report: <topic>

## 1. Original brief
...

## 2. Hypotheses tested
| ID | Statement | Status | Confidence |
|----|-----------|--------|------------|
| H1 | ...       | ...    | ...        |

## 3. What we did
- Data collected
- Experiments run
- Evaluation approach

## 4. Key findings
...

## 5. Limitations & risks
...

## 6. Recommendations
- Ready for handoff? yes / no / partially
- What should be built (if anything)
- What should *not* be built yet
- Open questions
```

## Discipline

- Separate **observations** from **interpretations**
- Be explicit about confidence and sample limitations
- Prefer "we do not yet know" over forced conclusions

## Anti-patterns

- Findings that go beyond what the evidence supports.
- Dropping limitations and risks to make the recommendation look stronger.
- Recommendations not tied to specific findings.
- Omitting hypotheses that were refuted.

---

Source: `lab-crew/skills/synthesis/SKILL.md`.
