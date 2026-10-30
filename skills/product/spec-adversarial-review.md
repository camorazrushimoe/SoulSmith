---
id: spec-adversarial-review
name: "Spec review: product lens"
description: >
  Review a spec.md before any implementation through the product lens: missing
  or untestable acceptance criteria, contradictions, hidden dependencies and
  unnamed owners, scope too big for one PR, and user-facing holes; publish a
  verdict comment on the GitHub issue or PR. Use when a spec needs adversarial
  review before coding. Not for infrastructure review (use spec-infra-review)
  or reviewing code (use code-diff-review).
domain: product
tags: [spec-review, adversarial-review, acceptance-criteria, product, github]
required_tools: [file-read, github]
optional_tools: []
input_schema:
  spec_path: string
  issue_or_pr_url: string
output_schema:
  verdict: "enum(approve|needs-changes|blocked)"
  blocking: list
  non_blocking: list
compatibility: [github]
version: 0.1.0
author: camorazrushimoe
---

# Spec review: product lens

Read `spec.md` and the originating GitHub issue or PR. Do not write application code.

Look for:

- missing or untestable acceptance criteria
- contradictions
- hidden dependencies and unnamed owners
- scope that cannot be finished in one PR
- user-facing holes (value, completeness, convenience)

Write only an evaluation. Do not propose a different product.

Publish on the GitHub issue/PR:

```
## Spec review

**Verdict:** approve | needs-changes | blocked
**Blocking (max 3):**
- ...
**Non-blocking:**
- ...
```

Done means the comment exists. The wrapper records the artifact URL on hook stop.

## Anti-patterns

- Writing application code during review.
- Proposing a different product instead of evaluating the spec.
- More than three blocking items, or blocking items that are really preferences.
- Finishing without posting the comment.

---

Source: `forgeyard/skills/adversarial-review/SKILL.md`.
