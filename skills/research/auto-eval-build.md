---
id: auto-eval-build
name: Automatic evaluation build
description: >
  Design and implement automatic evaluation (scripts, checks, LLM-as-judge
  with calibration) when the research question allows a machine-scorable
  answer. Use when an experiment needs repeatable scoring. Not when automatic
  metrics would be misleading (use grading-protocol-design) and not for
  choosing what to measure (use metric-design).
domain: research
tags: [evaluation, auto-eval, llm-as-judge, scoring, experiments]
required_tools: [python3, shell]
optional_tools: [llm-api]
input_schema:
  hypothesis: string
  metric_definition: string
output_schema:
  eval_script: string
  scores: table
compatibility: [lab-crew, hermes-agent]
version: 0.1.0
author: Sergey Ryabushko
---

# Automatic evaluation build

## Purpose

Design and implement automatic evaluation when the research question allows it.

## When to use

- A metric can be computed from data without human judgment
- Human grading exists and needs a scalable companion metric
- Agent outputs, rankings, classifications, or structured predictions must be scored repeatedly

## Process

1. Confirm the metric definition with Research Lead (and metric-design skill)
2. Implement a small, readable evaluation script
3. Run it on the experiment outputs
4. Report:
   - metric values
   - sample size
   - failure cases / edge cases
   - confidence caveats
5. Store evaluation code under `evaluation/` so it can be re-run

## Discipline

- Prefer simple, inspectable code over heavy frameworks at research stage
- Always show a few concrete examples of correct and incorrect cases
- If auto-eval disagrees with human grades, investigate instead of ignoring

## Anti-patterns

- Opaque scoring functions nobody can audit
- Reporting a single number without examples
- Changing the metric definition after seeing the score without recording the change

---

Source: `lab-crew/skills/auto-eval/SKILL.md`.
