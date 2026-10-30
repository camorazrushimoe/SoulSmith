---
id: plan-grill
name: Plan grilling interview
description: >
  Interview the user relentlessly about a plan, decision or idea, one question
  at a time, walking the decision tree and resolving dependencies until shared
  understanding is reached; optionally update CONTEXT.md and ADRs as you go.
  Use when the user wants to stress-test their thinking or says "grill me".
  Not for writing the spec afterwards (use spec-write) or for reviewing a
  finished spec (use spec-adversarial-review).
domain: product
tags: [interview, stress-test, planning, decision-tree, requirements]
required_tools: []
optional_tools: [file-edit]
input_schema:
  plan_or_idea: string
output_schema:
  resolved_decisions: list
  open_questions: list
compatibility: [hermes-agent, claude-code]
version: 0.1.0
author: camorazrushimoe
---

# Plan grilling interview

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled — the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Each question should be formatted like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Each round the user answers reshapes the tree — settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it — don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report — ask the rest of the frontier now. The _decisions_ are the user's — put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.

## Variant: grill with docs

Run the grilling session while applying `domain-model-refine`: update the CONTEXT.md glossary and record ADRs inline as terms and decisions get settled, so the interview leaves documentation behind.

## Anti-patterns

- Asking several questions at once.
- Asking the user things the codebase or docs already answer.
- Accepting vague answers instead of following up.
- Starting implementation or writing the spec mid-interview.

---

Source: `dev-crew/agents/tech-pm/skills/grilling/SKILL.md`; `dev-crew/agents/tech-pm/skills/grill-with-docs/SKILL.md (composite of grilling + domain-modeling, folded in)`.
