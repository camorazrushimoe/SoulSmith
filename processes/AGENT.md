# AGENT.md – Behavioral Contracts

This folder holds **behavioral contracts** for agents assembled by SoulSmith.

## 1. Concept

A behavioral contract is **not documentation**. It is a contract about behavior.

- A **skill** says: *"I can do X."* That is capability.
- A **contract** says: *"In situation Y, I do Z and never do W."* That is discipline.

Capability without discipline produces agents that can do the right thing but don't do it reliably. Contracts close that gap.

**Example**

- Skill: `git-branch-discipline` (the agent knows how to work with branches)
- Contract: *Never merge to `master`. Create a branch, commit, and wait for review. On `CHANGES_REQUESTED`, return to the same branch and push fixes there.*

## 2. Contract file format

One contract per file: `processes/<role>/<id>.md`. YAML frontmatter plus a Markdown body.

| Field | Required | Description |
|---|---|---|
| `id` | yes | Unique kebab-case id, e.g. `review-returned-fix-on-same-branch` |
| `role` | yes | Who follows it: `engineer`, `analyst`, `architect`, `qa`, `ops`, ... |
| `trigger` | yes | The situation that activates it, e.g. `branch created`, `review returned`, `deploy failed` |
| `steps` | yes | Short imperatives, in order |
| `escalation` | yes | Who to contact, and when |
| `stop_conditions` | yes | When the agent must stop and not continue on its own |
| `anti_patterns` | yes | What the agent must NOT do |
| `required_skills` | yes | Tags linking to skills in [`/skills`](../skills) |
| `version` | yes | Semantic version, start at `0.1.0` |
| `author` | yes | Who wrote or maintains it |

### Example

```markdown
---
id: review-returned-fix-on-same-branch
role: engineer
trigger: review returned with CHANGES_REQUESTED
steps:
  - Check out the same branch the PR was opened from
  - Address every review comment
  - Commit and push to that branch
  - Re-request review
escalation:
  - Contact the reviewer if a comment is ambiguous or conflicts with the spec
  - Contact the tech lead if fixes would change scope
stop_conditions:
  - Review comments contradict each other
  - The fix requires touching files outside the PR scope
anti_patterns:
  - Opening a new PR instead of updating the existing one
  - Merging to master yourself
  - Force-pushing over review history
required_skills: [git-branch-workflow, code-diff-review]
version: 0.1.0
author: camorazrushimoe
---

# Fix review feedback on the same branch

Why this contract exists, and any context the agent needs.
```

Write `steps` as imperatives. Keep each one short enough that a violation is obvious.

## 3. Relationship to skills

Contracts and skills form a **graph**, not two separate piles of files.

- A contract references the skills it depends on through `required_skills` tags.
- A skill may reference the processes it participates in.

When SoulSmith assembles an agent, it pulls in skills **and** the contracts that govern them, so the agent gets both capability and discipline.

## 4. Derivation order

Contracts come from two sources:

1. **Derived from skills.** These are scaffolding: cheap, predictable, and quick to write. Example: "never merge to master" comes straight from `git-branch-discipline`.
2. **Derived from incidents and failures.** These are the most valuable. Every failure points to a missing contract.

The order:

1. Skills first.
2. Then a first layer of contracts derived from skills.
3. Then contracts accumulate from real use, one failure at a time.

## 5. TODO: failure collection pipeline

> **TODO / future work. Not implemented.**

Once the service is running, Sergey will drop failure cases into this folder as case studies. Each case is input for a new or improved contract, and for improving the service overall.

- [ ] **TODO:** decide the storage format for failure cases (file layout, required fields, how a case links to the contract it produced). Not decided yet.
- [ ] **TODO:** define how a failure case becomes a contract (review step, who approves).
