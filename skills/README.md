# Skill Registry

This folder will hold the SoulSmith skill registry: the catalog of reusable skills the composer matches against when assembling an agent.

> **Status:** 45 skills collected from `dev-crew`, `lab-crew`, `agent-office`, `product-factory` and `forgeyard` (deduplicated by blob SHA; near-duplicates merged into one skill with variant notes).

## Manifest

Every skill ships with a YAML manifest (as frontmatter in its file). The manifest is what the matcher reads, so it must be precise. Field names are flat (no nesting).

| Field | Required | Description |
|---|---|---|
| `id` | yes | Unique, stable, kebab-case identifier, pattern `<object>-<action>` (e.g. `code-diff-review`) |
| `name` | yes | Short human-readable name |
| `description` | yes | What the skill does, when to use it, and when not to. Written for a matcher, not a reader |
| `domain` | yes | Primary domain: `dev`, `qa`, `research`, `product`, `ops` |
| `tags` | yes | 3 to 8 lowercase kebab-case keywords that help retrieval |
| `required_tools` | yes | List of tools the skill cannot work without (may be empty) |
| `optional_tools` | yes | List of tools that improve the result if present (may be empty) |
| `input_schema` | yes | What the skill expects as input |
| `output_schema` | yes | What the skill produces |
| `compatibility` | yes | List of runtimes, MCP servers, or platforms it is known to work with |
| `version` | yes | Semantic version of the skill |
| `author` | yes | Who wrote or maintains it |

### Body requirements

The Markdown body after the frontmatter holds the instructions the assembled agent follows. Every skill body **must** include an `## Anti-patterns` section listing concrete things not to do. A closing `Source:` note may record the origin repo and paths.

### Example manifest (illustrative)

```yaml
---
id: test-gap-analysis
name: Test gap analysis
description: >
  Given a pull request diff, identifies changed functions and branches that
  have no corresponding test changes. Use for QA review of code PRs. Not for
  documentation-only or config-only changes.
domain: qa
tags: [testing, coverage, pull-request, review]
required_tools: [github.get_pull_request_files]
optional_tools: [github.get_file_contents]
input_schema:
  pr_url: string
output_schema:
  untested_changes: list
compatibility: [github-mcp]
version: 0.1.0
author: soulsmith
---
```

## Index

### dev (8)

| id | name | domain | description | source repo(s) |
|---|---|---|---|---|
| [`agent-doc-write`](dev/agent-doc-write.md) | Writing documents for agents | dev | Reference for writing documents an agent consumes: SKILL.md files, AGENTS.md / CLAUDE.md, and docs reached by pointers. | dev-crew |
| [`dev-env-deploy-verify`](dev/dev-env-deploy-verify.md) | Dev sandbox deploy and verify | dev | Bring a project up with docker-compose on the developer's sandbox network (dev-env), hit its health endpoint and smoke-test the changed path before opening a pull request. | dev-crew |
| [`git-branch-workflow`](dev/git-branch-workflow.md) | Git feature-branch workflow | dev | Rules for working in git on a feature branch: branch naming feature/&lt;ticket&gt;-&lt;slug&gt;, small English commits, push and open a pull request, never commit or push to main/master, and a list of blocked destructive commands. | agent-office, dev-crew, forgeyard |
| [`merge-conflict-resolve`](dev/merge-conflict-resolve.md) | Merge conflict resolution | dev | Resolve an in-progress git merge or rebase conflict: inspect state, read the commits, PRs and tickets behind each side, resolve each hunk preserving both intents, run typecheck/tests/format, then finish the merge or rebase. | agent-office, dev-crew, forgeyard |
| [`module-interface-design`](dev/module-interface-design.md) | Module interface design (deep modules) | dev | Reference vocabulary and rules for designing deep modules: small interface, large implementation, placed at a clean seam and tested through that interface. | agent-office, dev-crew, forgeyard |
| [`prototype-build`](dev/prototype-build.md) | Throwaway prototype build | dev | Build throwaway code that answers one design question: either a single-file HTML logic/state-machine demo a non-developer can drive, or several radically different UI variations on one route switchable by a URL param. | agent-office, dev-crew |
| [`spec-implement`](dev/spec-implement.md) | Implement from spec or tickets | dev | Implement the work described in a spec or set of tickets on the current feature branch: test-first at pre-agreed seams, frequent typechecks and single-file test runs, full suite once at the end, self-review, then commit. | agent-office, dev-crew, forgeyard |
| [`tdd-cycle-run`](dev/tdd-cycle-run.md) | Test-driven development loop | dev | Red-green test-driven development: agree seams with the user, write one failing test through the public interface, write the minimum code to pass, repeat in vertical slices. | agent-office, dev-crew, forgeyard |

### qa (4)

| id | name | domain | description | source repo(s) |
|---|---|---|---|---|
| [`bug-diagnose`](qa/bug-diagnose.md) | Bug diagnosis loop | qa | Disciplined diagnosis for hard bugs and performance regressions: build a fast deterministic feedback loop first, reproduce, form ranked falsifiable hypotheses, instrument, then fix behind a regression test and clean up. | dev-crew, forgeyard |
| [`code-diff-review`](qa/code-diff-review.md) | Code diff review (standards + spec) | qa | Review the changes since a fixed point (commit, branch, tag or merge-base) on two axes: Standards (does the diff follow the repo's documented coding standards) and Spec (does it do what the originating issue or spec asked). | agent-office, dev-crew, forgeyard |
| [`pr-cluster-verify`](qa/pr-cluster-verify.md) | PR verification on cluster | qa | On a remote SSH host, check out a pull request's head SHA, run `make qa` as the gate, optionally run a smoke test, and post a QA-on-cluster comment with a merge or no-merge verdict. | forgeyard |
| [`qa-report-write`](qa/qa-report-write.md) | QA report | qa | Write a structured QA report after testing a change: scope, PASS/FAIL/NEEDS CHANGES result, tests run, bugs with severity and reproduction steps, coverage gaps and a merge verdict; post it to the ticket and the bus. | dev-crew, forgeyard |

### product (10)

| id | name | domain | description | source repo(s) |
|---|---|---|---|---|
| [`decision-map-plan`](product/decision-map-plan.md) | Decision map planning (wayfinder) | product | Plan work too large for one agent session as a shared map of decision tickets on the issue tracker, with a destination, decisions so far, fog of war and out-of-scope list, then resolve the decision tickets one at a time. | dev-crew |
| [`domain-model-refine`](product/domain-model-refine.md) | Domain model and ADRs | product | Build and sharpen a project's domain model: maintain the CONTEXT.md glossary, challenge fuzzy or overloaded terms, and record architecture decisions as ADRs in a fixed format. | dev-crew, forgeyard |
| [`intake-discovery-gate`](product/intake-discovery-gate.md) | Intake discovery gate | product | Mandatory operator checkpoint after a request arrives: timeboxed triage (normalize the request, Linear lookup, surface scan, stakeholder questions), then an Intake Brief with understanding, what was checked, questions, depth options and a Linear plan, ending the turn until the operator replies. | product-factory |
| [`issue-triage`](product/issue-triage.md) | Issue and PR triage | product | Move incoming issues and external PRs through a triage state machine: categorise, verify, grill if needed, mark out-of-scope, and write agent-ready briefs. | dev-crew, forgeyard |
| [`plan-grill`](product/plan-grill.md) | Plan grilling interview | product | Interview the user relentlessly about a plan, decision or idea, one question at a time, walking the decision tree and resolving dependencies until shared understanding is reached; optionally update CONTEXT.md and ADRs as you go. | dev-crew |
| [`spec-adversarial-review`](product/spec-adversarial-review.md) | Spec review: product lens | product | Review a spec.md before any implementation through the product lens: missing or untestable acceptance criteria, contradictions, hidden dependencies and unnamed owners, scope too big for one PR, and user-facing holes; publish a verdict comment on the GitHub issue or PR. | forgeyard |
| [`spec-pipeline-drive`](product/spec-pipeline-drive.md) | Product Factory spec pipeline | product | Drive a Product Factory spec pipeline (intake, understand-system, research, shape, spec, human review gate, finalize) as the Technical Product Manager inside an Agent Office deployment: publish events on the Redis office bus, hand work to sibling agents over HMAC webhook doors, and maintain the per-project LLM Wiki. | agent-office |
| [`spec-ticket-breakdown`](product/spec-ticket-breakdown.md) | Spec to tracer-bullet tickets | product | Break a plan, spec or conversation into thin vertical tracer-bullet tickets under one Linear Project, each declaring its blocking edges, and publish them to the tracker. | dev-crew |
| [`spec-write`](product/spec-write.md) | Spec from conversation | product | Turn what has already been discussed into a spec and publish it to the project's issue tracker, without a fresh interview. | dev-crew, forgeyard |
| [`work-intent-gate`](product/work-intent-gate.md) | Work intent alignment gate | product | Before a team starts a ticket or a series, reconstruct the intent chain (stated goal, routing decision, ticket breakdown, planned execution), check it against known drift patterns, and return ALIGNED, DRIFTED or UNCLEAR, with authority to stop work and escalate to the customer. | agent-office |

### research (16)

| id | name | domain | description | source repo(s) |
|---|---|---|---|---|
| [`auto-eval-build`](research/auto-eval-build.md) | Automatic evaluation build | research | Design and implement automatic evaluation (scripts, checks, LLM-as-judge with calibration) when the research question allows a machine-scorable answer. | lab-crew |
| [`data-source-parse`](research/data-source-parse.md) | Quick research parser | research | Build lightweight, disposable parsers and collectors for research data sources (web pages, APIs, public datasets, files), respecting rate limits and terms. | lab-crew |
| [`dataset-collect`](research/dataset-collect.md) | Experiment dataset collection | research | Collect and organize temporary datasets needed for an experiment, with provenance and a README, without building production data infrastructure. | lab-crew |
| [`eval-report-write`](research/eval-report-write.md) | Evaluation report section | research | Write the evaluation section for a hypothesis or experiment (setup, metrics, results, caveats) in a fixed template that the Research Lead can drop into the Research Report and handoff package. | lab-crew |
| [`eval-result-critique`](research/eval-result-critique.md) | Evaluation result critique | research | Actively look for ways an evaluation or experiment result could be misleading: leakage, small or biased samples, metric gaming, confounds, cherry-picking, judge bias. | lab-crew |
| [`experiment-brief-write`](research/experiment-brief-write.md) | Research Engineer brief | research | Write a clear, actionable brief for the Research Engineer so an experiment can start without ambiguity: hypothesis, scope, data, deliverables, budget and stop conditions. | lab-crew |
| [`experiment-design`](research/experiment-design.md) | Experiment design | research | Design the cheapest viable experiment that can produce evidence for or against a stated hypothesis: variables, data, procedure, success and kill criteria, budget. | lab-crew |
| [`experiment-notes-write`](research/experiment-notes-write.md) | Reproducible experiment notes | research | Leave enough scripts, commands, versions and notes that another agent or human can re-run or audit an experiment. | lab-crew |
| [`exploratory-analysis-run`](research/exploratory-analysis-run.md) | Exploratory analysis | research | Run lightweight analysis (notebooks or scripts, descriptive stats, quick plots) that produces evidence for or against a hypothesis. | lab-crew |
| [`grading-protocol-design`](research/grading-protocol-design.md) | Human grading protocol | research | Design clear human grading protocols (rubric, examples, grader instructions, agreement checks) when automatic metrics are insufficient or misleading. | lab-crew |
| [`hypothesis-craft`](research/hypothesis-craft.md) | Hypothesis crafting | research | Turn a fuzzy idea or research brief into one or more explicit, testable hypotheses with expected evidence and falsification criteria. | lab-crew |
| [`hypothesis-status-track`](research/hypothesis-status-track.md) | Hypothesis status tracking | research | Keep the living status of hypotheses and experiments accurate and easy to scan in one canonical tracking file. | lab-crew |
| [`metric-design`](research/metric-design.md) | Metric design | research | Choose and define metrics that answer the research question rather than the easiest number to compute, including definition, computation, baseline and known failure modes. | lab-crew |
| [`research-handoff-package`](research/research-handoff-package.md) | Research handoff package | research | Package research findings so DevCrew or another product team can act on them without the original researchers: recommendation, evidence, limitations, artifacts and reproduction steps. | lab-crew |
| [`research-report-synthesize`](research/research-report-synthesize.md) | Research report synthesis | research | Turn experiment results and evaluation outcomes into a Research Report with a fixed minimum structure: brief, hypotheses tested, what was done, key findings, limitations and risks, recommendations. | lab-crew |
| [`topic-research`](research/topic-research.md) | Primary-source topic research | research | Investigate a question against high-trust primary sources (official docs, specs, source code) and capture the findings with citations as a Markdown file in the repo. | agent-office, dev-crew |

### ops (7)

| id | name | domain | description | source repo(s) |
|---|---|---|---|---|
| [`agent-task-dispatch`](ops/agent-task-dispatch.md) | Agent task dispatch | ops | Hand a task to another crew agent (developer, qa, tech-pm, devops) through its webhook door with the crew-send client, using a fixed message format that starts with the Linear ticket ID. | dev-crew |
| [`github-portfolio-sweep`](ops/github-portfolio-sweep.md) | GitHub portfolio sweep | ops | Scrum Master routine: collect repo activity and open issues/PRs across a GitHub org for the last N days, group repos as active/quiet/silent, classify issues into buckets (bug, feature, tech-debt, performance, docs, question), propose labels for human approval, and update the portfolio memory file. | agent-office |
| [`linear-issue-manage`](ops/linear-issue-manage.md) | Linear issue management | ops | Create, move, label, assign and read Linear tickets and Projects through the Linear GraphQL API, following the crew's state and labeling conventions. | agent-office, dev-crew |
| [`session-handoff-write`](ops/session-handoff-write.md) | Session handoff document | ops | Compact the current conversation into a handoff document a fresh agent session can continue from, saved to the OS temp directory, with suggested skills, references to existing artifacts instead of copies, and secrets redacted. | dev-crew |
| [`spec-infra-review`](ops/spec-infra-review.md) | Spec review: infrastructure lens | ops | Adversarial review of an OpenSpec change or spec through the infrastructure lens (packaging, persistence, migrations, scheduling, secrets, observability, deployability), posting a verdict on the GitHub issue in the format it requests. | dev-crew |
| [`staging-deploy`](ops/staging-deploy.md) | Staging deploy | ops | Deploy reviewed and merged code to the staging-env pre-prod network: confirm the request, run idempotent migrations, start or update the service, health-check, and report explicitly. | dev-crew |
| [`staging-env-manage`](ops/staging-env-manage.md) | Staging environment management | ops | Own the staging-env pre-prod Docker network end to end: topology (dev-env vs staging-env vs crew), onboarding a project's compose file, keeping the project's declared stack deployable and observable. | dev-crew |

See [AGENT.md](AGENT.md) for how to add a skill.
