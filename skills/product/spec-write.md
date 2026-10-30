---
id: spec-write
name: Spec from conversation
description: >
  Turn what has already been discussed into a spec and publish it to the
  project's issue tracker, without a fresh interview. Use when the
  conversation already holds the decisions and they need to become a spec. Not
  for discovering requirements (use plan-grill or intake-discovery-gate) or
  splitting the spec into tickets (use spec-ticket-breakdown).
domain: product
tags: [spec, requirements, synthesis, issue-tracker, prd]
required_tools: [issue-tracker]
optional_tools: [file-edit, github, linear]
input_schema:
  conversation_context: string
output_schema:
  spec: markdown
  issue_url: string
compatibility: [hermes-agent, linear, github]
version: 0.1.0
author: camorazrushimoe
---

# Spec from conversation

This skill takes the current conversation context and codebase understanding and produces a spec. Do NOT interview the user — just synthesize what you already know.

The issue tracker and triage label vocabulary should have been provided to you — run `/setup-matt-pocock-skills` if not.

## Process

1. Explore the repo to understand the current state of the codebase, if you haven't already. Use the project's domain glossary vocabulary throughout the spec, and respect any ADRs in the area you're touching.

2. Sketch out the seams at which you're going to test the feature. Existing seams should be preferred to new ones. Use the highest seam possible. If new seams are needed, propose them at the highest point you can. The fewer seams across the codebase, the better - the ideal number is one.

Check with the user that these seams match their expectations.

3. Write the spec using the template below, then publish it to the project issue tracker. Apply the `ready-for-agent` triage label - no need for additional triage.

<spec-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A LONG, numbered list of user stories. Each user story should be in the format of:

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

This list of user stories should be extremely extensive and cover all aspects of the feature.

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified
- The interfaces of those modules that will be modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it within the relevant decision and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.

## Testing Decisions

A list of testing decisions that were made. Include:

- A description of what makes a good test (only test external behavior, not implementation details)
- Which modules will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope

A description of the things that are out of scope for this spec.

## Further Notes

Any further notes about the feature.

</spec-template>

## Compact variant (forgeyard)

Writes `spec.md` with the minimum sections Problem, Goals / non-goals, Acceptance criteria, Constraints, Open questions, and publishes to GitHub instead of Linear.

Write `spec.md` in the project from what is already known (issue body, comments, repo).
Do not interview. If a fact is missing, list it under Open questions.

Minimum sections:

- Problem
- Goals / non-goals
- Acceptance criteria (testable)
- Constraints
- Open questions

Commit on a feature branch if the checkout is a git repo, and point the GitHub issue at the file.
Do not start implementation in the same run.

## Anti-patterns

- Re-interviewing the user when the decisions are already in the conversation.
- Inventing requirements nobody discussed.
- Acceptance criteria that cannot be tested.
- Leaving open questions out instead of listing them.

---

Source: `dev-crew/agents/tech-pm/skills/to-spec/SKILL.md`; `forgeyard/skills/to-spec/SKILL.md (condensed rewrite, merged)`. Provenance: the source references `/setup-matt-pocock-skills`, so it appears adapted from Matt Pocock's public skills; confirm the upstream license before redistribution.
