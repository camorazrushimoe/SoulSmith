---
id: eval-result-critique
name: Evaluation result critique
description: >
  Actively look for ways an evaluation or experiment result could be
  misleading: leakage, small or biased samples, metric gaming, confounds,
  cherry-picking, judge bias. Use before a result is reported or acted on. Not
  for designing the evaluation (use auto-eval-build or
  grading-protocol-design) or writing it up (use eval-report-write).
domain: research
tags: [evaluation, critique, validity, bias, red-team]
required_tools: [file-read]
optional_tools: [python3]
input_schema:
  result: string
  eval_setup: string
output_schema:
  critique: markdown
  severity: string
compatibility: [lab-crew, hermes-agent]
version: 0.1.0
author: Sergey Ryabushko
---

# Evaluation result critique

## Purpose

Actively look for ways an evaluation (or experiment result) could be misleading.

## When to use

- Before treating results as conclusive
- When reviewing experiment design
- When a metric looks "too good" or surprisingly clean

## Common failure modes to check

- **Leakage** — information from the future or from the label leaked into features
- **Selection bias** — the sample is not representative of the claimed population
- **Cherry-picking** — only favorable slices are reported
- **Metric mismatch** — the metric does not measure the actual hypothesis
- **Tiny sample** — confidence is overstated
- **Confounding** — an alternative explanation fits the data equally well
- **Non-reproducibility** — results depend on hidden choices or unstable code

## Output

Write a short critique note:

```markdown
## Evaluation Critique

**Target:** <experiment or metric>
**Risks found:**
- ...
**Severity:** low / medium / high
**Recommendation:** accept with caveats / re-run / redesign measurement
```

## Discipline

- Be specific. "Might be biased" is not enough.
- Prefer actionable recommendations over pure skepticism.

## Anti-patterns

- Rubber-stamping a result because it confirms the hypothesis.
- Listing generic caveats instead of checking the specific failure modes.
- Raising concerns without saying what evidence would resolve them.
- Rewriting the experiment instead of critiquing it.

---

Source: `lab-crew/skills/evaluation-critique/SKILL.md`.
