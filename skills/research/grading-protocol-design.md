---
id: grading-protocol-design
name: Human grading protocol
description: >
  Design clear human grading protocols (rubric, examples, grader instructions,
  agreement checks) when automatic metrics are insufficient or misleading. Use
  when quality must be judged by people. Not when an automatic metric answers
  the question (use auto-eval-build).
domain: research
tags: [human-grading, rubric, annotation, evaluation, inter-rater-agreement]
required_tools: [file-write]
optional_tools: []
input_schema:
  task: string
  quality_dimensions: list
output_schema:
  grading_protocol: markdown
compatibility: [lab-crew, hermes-agent]
version: 0.1.0
author: Sergey Ryabushko
---

# Human grading protocol

## Purpose

Design clear human grading protocols when automatic metrics are insufficient or misleading.

## When to use

- The research question involves quality, relevance, usefulness, or other subjective judgments
- Auto-eval exists but needs calibration or validation
- Building evaluation for agent outputs, summaries, recommendations, etc.

## What a good protocol includes

1. **Grading goal** — what decision the grades will support
2. **Scale** — binary, Likert, rubric categories (keep it simple)
3. **Definitions** — precise meaning of each score
4. **Examples** — positive, negative, and borderline cases
5. **Process** — how graders work, how disagreements are handled
6. **Quality checks** — inter-rater agreement, spot checks, gold examples

## Template

```markdown
## Human Grading Protocol: <name>

**Linked hypothesis / metric:** ...
**Scale:** ...
**Definitions:**
- Score 1: ...
- Score 2: ...
**Examples:**
- Good: ...
- Bad: ...
- Borderline: ...
**Process:** ...
**Agreement check:** ...
```

## Anti-patterns

- Vague labels ("good / bad") without definitions
- No examples
- No plan for disagreement between graders
- Changing the rubric after seeing results without recording it

---

Source: `lab-crew/skills/human-grading/SKILL.md`.
