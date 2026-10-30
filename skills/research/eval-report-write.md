---
id: eval-report-write
name: Evaluation report section
description: >
  Write the evaluation section for a hypothesis or experiment (setup, metrics,
  results, caveats) in a fixed template that the Research Lead can drop into
  the Research Report and handoff package. Use after an evaluation has run.
  Not for critiquing whether the result is trustworthy (use
  eval-result-critique) or writing the whole report (use
  research-report-synthesize).
domain: research
tags: [evaluation, reporting, results, research-report]
required_tools: [file-write]
optional_tools: []
input_schema:
  experiment: string
  results: table
output_schema:
  evaluation_section: markdown
compatibility: [lab-crew, hermes-agent]
version: 0.1.0
author: Sergey Ryabushko
---

# Evaluation report section

## Purpose

Write clear evaluation sections that Research Lead can use in the Research Report and handoff package.

## When to use

- After metrics or human grades are available
- When synthesizing evaluation outcomes for a hypothesis

## What to include

1. Which hypothesis / claim was measured
2. Metric definitions (short)
3. Results with sample size
4. Concrete examples (good / bad / borderline)
5. Risks and caveats (from evaluation-critique)
6. Recommendation: supports / weakens / inconclusive for the hypothesis

## Template

```markdown
## Evaluation — <hypothesis or experiment>

**Metric(s):** ...
**Sample size:** ...
**Result:** ...
**Examples:**
- ...
**Caveats:** ...
**Verdict for hypothesis:** supports | weakens | inconclusive
```

## Discipline

- Be readable by someone who did not run the evaluation
- Do not hide uncertainty
- Prefer "inconclusive" over forced confidence

## Anti-patterns

- Reporting only the headline number without setup, sample size or caveats.
- Mixing interpretation into the results table.
- Omitting negative or null results.
- Results that cannot be traced back to scripts and data.

---

Source: `lab-crew/skills/eval-reporting/SKILL.md`.
