---
id: research-handoff-package
name: Research handoff package
description: >
  Package research findings so DevCrew or another product team can act on them
  without the original researchers: recommendation, evidence, limitations,
  artifacts and reproduction steps. Use when research is finished and moves to
  implementation. Not for an agent-to-agent session handoff (use
  session-handoff-write).
domain: research
tags: [handoff, research, recommendations, product-team]
required_tools: [file-write]
optional_tools: []
input_schema:
  research_report: string
output_schema:
  handoff_package: markdown
compatibility: [lab-crew, hermes-agent]
version: 0.1.0
author: Sergey Ryabushko
---

# Research handoff package

## Purpose

Package research findings into a form that DevCrew (or another product team) can act on without needing the original researchers.

## When to use

- Research Report concludes that something is worth building
- Manager asks for a product-ready package

## Minimum handoff package

Create (or update) `workspace/<topic>/handoff/` with:

1. **SUMMARY.md** — short decision + why
2. **validated-hypotheses.md** — what was supported / rejected
3. **data-sources.md** — recommended sources, collection notes, limitations
4. **evaluation.md** — metrics and evaluation approach that should carry into the product
5. **requirements-sketch.md** — draft requirements or OpenSpec-style notes (optional but valuable)
6. **open-questions.md** — risks and unknowns

## Discipline

- Write for a reader who was not in the research loop
- Do not assume shared context
- Prefer concrete recommendations over vague "further research needed" when a decision is possible
- Explicitly state what LabCrew is *not* handing off (no production code, no long-lived infra)

## Anti-patterns

- Handing off conclusions without the evidence and limitations behind them.
- Artifacts that only run on the researcher's machine.
- Recommendations the receiving team cannot act on without asking the researchers.
- Leaving out what was tried and did not work.

---

Source: `lab-crew/skills/handoff/SKILL.md`.
