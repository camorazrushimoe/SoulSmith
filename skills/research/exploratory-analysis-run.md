---
id: exploratory-analysis-run
name: Exploratory analysis
description: >
  Run lightweight analysis (notebooks or scripts, descriptive stats, quick
  plots) that produces evidence for or against a hypothesis. Use when data is
  available and a quick evidence check is needed. Not for building automatic
  evaluation harnesses (use auto-eval-build) or production analytics.
domain: research
tags: [analysis, exploratory, data, evidence, notebooks]
required_tools: [python3]
optional_tools: [shell]
input_schema:
  hypothesis: string
  dataset_path: string
output_schema:
  findings: markdown
  artifacts: list
compatibility: [lab-crew, hermes-agent]
version: 0.1.0
author: Sergey Ryabushko
---

# Exploratory analysis

## Purpose

Run lightweight analysis that produces evidence for or against a hypothesis.

## When to use

- Data has been collected for an experiment
- Research Lead or Evaluation Specialist needs intermediate findings

## Principles

1. Start with simple counts, distributions and breakdowns
2. Prefer transparent methods over complex models at the research stage
3. Always report sample size and obvious biases
4. Separate descriptive findings from causal claims
5. Leave notebooks/scripts that can be re-run

## Typical outputs

- Summary tables
- Simple plots (distributions, time trends, group comparisons)
- Correlation / association checks (with caveats)
- Qualitative examples that illustrate patterns
- Explicit "what this does *not* show"

## Anti-patterns

- Jumping to a complex model before basic distributions are understood
- Reporting only the flattering slice of results
- Treating correlation as causation in the write-up
- Leaving analysis that only the original author can interpret

---

Source: `lab-crew/skills/exploratory-analysis/SKILL.md`.
