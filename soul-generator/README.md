# Soul Generator

> **Status:** concept only. Not implemented.

A *soul* is a domain-specific Markdown document (`SOUL.md`) that gives an assembled agent the judgment of a good practitioner in its field. Skills say *what* an agent can do; the soul says *how it should think and behave while doing it*.

## What a soul contains

- **Role and stance** – who the agent is and what it optimizes for
- **Best practices** – the domain's established working principles
- **Quality bar** – what "done" and "good" look like
- **Anti-patterns** – common mistakes to avoid
- **Escalation rules** – when to stop, ask, or hand off
- **Communication style** – how to report results in this domain

## Initial domains

| Domain | Focus |
|---|---|
| `dev` | Small reviewable changes, tests with code, readable diffs, no silent breaking changes |
| `qa` | Risk-based testing, reproducible bug reports, coverage of edge cases, regression thinking |
| `research` | Source quality, citing evidence, separating facts from inference, stating uncertainty |
| `ops` | Safety first, reversible changes, runbooks, observability, blast-radius awareness |

## How generation is planned to work

1. Take the request (role, task, context) and the matched skills.
2. Start from a curated **base soul** for the domain.
3. Specialize it with request context (stack, team conventions, constraints).
4. Add skill-specific guidance contributed by the matched skills.
5. Output a single `SOUL.md` attached to the assembled agent.

## Planned structure

```
soul-generator/
  README.md          – this document
  base/              – curated base souls per domain
    dev.md
    qa.md
    research.md
    ops.md
  templates/         – section templates used during generation
  generator/         – generation logic (later)
```

See [../ROADMAP.md](../ROADMAP.md) for when this is scheduled.
