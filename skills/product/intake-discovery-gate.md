---
id: intake-discovery-gate
name: Intake discovery gate
description: >
  Mandatory operator checkpoint after a request arrives: timeboxed triage
  (normalize the request, Linear lookup, surface scan, stakeholder questions),
  then an Intake Brief with understanding, what was checked, questions, depth
  options and a Linear plan, ending the turn until the operator replies.
  Includes Linear project/ticket conventions and the cheap-work bypass rule.
  Use at intake before any deep research, analysis or spec work. Not for
  writing the spec itself (use spec-write).
domain: product
tags: [intake, discovery, triage, operator-checkpoint, linear, brief]
required_tools: [linear]
optional_tools: [web-search, file-read]
input_schema:
  request: string
output_schema:
  intake_brief: markdown
  linear_plan: string
compatibility: [hermes-agent, linear, product-factory]
version: 0.1.0
author: camorazrushimoe
---

# Intake discovery gate

## When to use
A request arrived. Before ANY deep work (system analysis, research, spec
writing), do ONLY lightweight triage and come back with an Intake Brief.
Deep work starts only after the operator replies.

## Procedure

### 1. Triage (timebox: ~15 min of tool work total)
- Normalize the request; extract entities and the Problem Hypothesis.
- **Linear lookup**: search for an existing project matching this
  client/product. If none: propose a new one in the brief. NEVER close a
  project — project closure is a human decision only.
- **Surface scan** of available material: repo README/structure overview,
  public facts. NO cloning-and-auditing, NO full research.
- Prepare stakeholder questions.

### 2. Intake Brief (this ENDS your turn)
Structure:

```
INTAKE BRIEF — <project>            (reflects repo/Linear state as of <date>)
1. UNDERSTANDING      how I understood the request (2–5 sentences)
2. WHAT I CHECKED     Linear state / repo surface / external facts (+sources)
3. QUESTIONS FOR YOU  question + why it matters + how the answer changes work
4. DEPTH OPTIONS      option A/B/C: what I'd do, effort, expected output
5. RECOMMENDATION     my pick + why (one line)
6. LINEAR             reuse project X | propose new project Y + INVESTIGATION ticket
```

Publish `intake.brief` on the office bus, post the brief as your reply,
create/update the `INVESTIGATION` Linear ticket — the ticket comment is the
CANONICAL copy of the brief; chat reply and bus event are mirrors.

Then STOP. Do not run understand-system / research / shape-and-spec, do not
delegate them, until the operator replies in this session.

### 3. Cheap-work bypass
If the request is trivially cheap — a single lookup or short factual answer
within the same ~15-minute timebox: just do it, report what was done and
assumed, and record it as an INVESTIGATION ticket closed with the outcome.
Gate is mandatory when anything is ambiguous, exceeds the timebox, needs
stakeholder input, or has unknown cost drivers. When in doubt, use the gate.

## Anti-patterns (violations)
- Running the full pipeline "to save a roundtrip"
- Writing spec drafts before scope agreement
- Closing or archiving Linear projects yourself
- Asking zero questions when material facts are missing

---

Source: `product-factory/agents/technical-product-manager/skills/discovery-gate/ (SKILL.md body, DESCRIPTION.md frontmatter)`.
