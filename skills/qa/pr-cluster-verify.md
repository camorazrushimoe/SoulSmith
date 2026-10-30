---
id: pr-cluster-verify
name: PR verification on cluster
description: >
  On a remote SSH host, check out a pull request's head SHA, run `make qa` as
  the gate, optionally run a smoke test, and post a QA-on-cluster comment with
  a merge or no-merge verdict. Use when a PR must be verified on real
  infrastructure before merge. Not for local unit testing and never for
  merging or pushing to main.
domain: qa
tags: [qa, ssh, pull-request, make-qa, verdict]
required_tools: [ssh, git, github]
optional_tools: [make]
input_schema:
  pr_url: string
  head_sha: string
  cluster_host: string
output_schema:
  verdict: "enum(merge|no-merge)"
  pr_comment_url: string
compatibility: [github, linux]
version: 0.1.0
author: camorazrushimoe
---

# PR verification on cluster

1. Do not merge. Do not push to main.
2. Via SSH: fetch and checkout the PR head sha.
3. Run `make qa` in the repo root. That is the v0 gate.
4. Optional smoke skill after that, if present.
5. Comment on the PR:

```
## QA on cluster

**Cluster:** user@host
**Deployed sha:** …
**Tests:** make qa …
Verdict: merge
```

or `Verdict: no-merge`.

## Anti-patterns

- Testing the branch tip instead of the exact PR head SHA.
- Merging the PR or pushing to main after a passing run.
- Posting a verdict without the deployed SHA and the `make qa` result.
- Skipping `make qa` and relying only on an ad-hoc smoke test.

---

Source: `forgeyard/skills/qa-on-cluster/SKILL.md`.
