# SoulSmith

**Describe the agent you need. SoulSmith forges it.**

SoulSmith is an agent composer. Instead of hand-building every agent from scratch, you describe what you need (a role, a task, some context) and SoulSmith assembles a live agent from a registry of reusable parts:

- **Skills** – focused, well-described capabilities ("triage a failing CI run", "write a regression test plan")
- **Tools** – the concrete actions a skill can take
- **Connectors** – MCP servers and integrations (GitHub, issue trackers, docs, data sources)
- **Soul** – a domain-specific `SOUL.md` that carries the best practices, judgment, and working style for the role

The result is an agent that knows *what* to do, *how* to do it, and *how a good practitioner in that domain would think about it*.

## Why this exists

SoulSmith started from a very practical pain. Setting up agent flows for QA, development, delivery, and research meant doing the same work over and over: picking skills, wiring GitHub tools, writing long system prompts full of best practices, and then tweaking all of it every time the task changed slightly.

Most of that work is composition, not invention. The skills already exist. The best practices are known. What's missing is a reliable way to put the right pieces together for the job at hand. SoulSmith is an attempt to make that composition step automatic, repeatable, and inspectable.

## How it works

```
request  ->  skill matching  ->  soul generation  ->  assembled agent
```

1. **Request.** You describe the agent: role, task, constraints, environment.
2. **Skill matching.** SoulSmith matches the request against the skill registry using each skill's manifest (description, domain, tags, required tools, input/output schemas).
3. **Soul generation.** A domain soul is generated or selected: the best practices and working principles for that role (dev, QA, research, ops, ...).
4. **Assembly.** Skills, tools, connectors, and soul are combined into a ready-to-run agent definition.

## Quick example

**Request**

> I need a QA agent for a TypeScript monorepo on GitHub. It should look at new PRs, find untested changes, and propose test cases.

**What SoulSmith would assemble (illustrative)**

```yaml
agent:
  role: QA engineer
  soul: souls/qa.md
  skills:
    - github-pr-diff-review
    - test-gap-analysis
    - test-case-proposal
  connectors:
    - github-mcp
  tools_required:
    - github.get_pull_request
    - github.get_pull_request_files
    - github.create_pull_request_review
```

## Repository layout

```
README.md            – you are here
ROADMAP.md           – phased plan
skills/              – skill registry (manifests + docs)
  README.md          – registry index and manifest schema
  AGENT.md           – guide for agents (and humans) adding skills
soul-generator/      – concept and planned structure for domain souls
```

## Status

**Early. Structure only.** There is no working composer yet. This repository currently holds the project statement, the shape of the skill registry, the soul generator concept, and the roadmap. See [ROADMAP.md](ROADMAP.md) for what comes next.

Feedback and ideas are welcome via issues.
