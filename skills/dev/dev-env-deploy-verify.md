---
id: dev-env-deploy-verify
name: Dev sandbox deploy and verify
description: >
  Bring a project up with docker-compose on the developer's sandbox network
  (dev-env), hit its health endpoint and smoke-test the changed path before
  opening a pull request. Use after finishing a code change to confirm the
  service actually boots and runs, not just that tests pass. Not for staging
  or production deploys (use staging-deploy) and not for environments owned by
  devops.
domain: dev
tags: [docker-compose, dev-env, sandbox, smoke-test, health-check, pre-pr]
required_tools: [shell, docker-compose]
optional_tools: [curl]
input_schema:
  project_path: string
  health_endpoint: string
output_schema:
  verification_result: string
  health_output: string
compatibility: [docker, hermes-agent]
version: 0.1.0
author: camorazrushimoe
---

# Dev sandbox deploy and verify

You own `dev-env` — your sandbox ("Dev Cluster"). Bring the project up there and
verify it actually runs before you open a PR. This is your local Docker: build,
test, iterate, break freely.

## When to use

- You finished a change and want to confirm it actually boots/runs (not just passes tests).
- You need to iterate: bring up → test → change → bring up again.

## Workflow

1. Read the project's `workspace/<project>/compose.yml` and README (required env
   vars, seed steps, health endpoint).
2. Bring the project up on `dev-env`:
   ```bash
   cd /workspace/<project>
   docker-compose -f compose.yml up -d --build
   ```
   The compose must attach to `dev-env` as an external network
   (`networks: dev-env: { external: true, name: dev-crew-dev-env }`). Add it if missing.
3. Verify health — hit the project's health endpoint (e.g. `curl http://localhost:8080/healthz`
   or the service's `/healthz` on the network).
4. Exercise the change against the running service where possible (smoke the path you built).
5. Tear down or leave running — `dev-env` is a sandbox; break freely.

## Rules

- **Never deploy to `staging-env`** — that is devops's pre-prod gate. Ask devops for staging.
- Don't commit secrets; env vars come from the project's `.env` (gitignored).
- The image provides the `docker-compose` (standalone v2) binary. If it is missing,
  use plain `docker` commands or ask the manager to run compose from the host.
- Record the verification result (command + health output) in the PR description.

## Anti-patterns

- Deploying to staging-env from the developer role; staging is the devops pre-prod gate.
- Declaring a change verified because unit tests pass, without booting the service.
- Committing secrets or a populated .env to make the compose file work.
- Opening the PR without recording the verification command and health output.

---

Source: `dev-crew/agents/developer/skills/deploy-dev/SKILL.md`.
