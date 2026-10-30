# Roadmap

SoulSmith is built in phases. Each phase should be usable on its own before the next one starts.

## Phase 1 – Skill registry

- Skill manifest metadata schema (YAML frontmatter)
- First set of skills across dev, QA, research, and ops
- `skills/AGENT.md` contributor guide for agents and humans adding skills
- Validation for manifests (required fields, naming, uniqueness)

## Phase 2 – MCP server

- MCP server exposing a `compose_agent` tool: request in, assembled agent definition out
- Simple matching over skill manifests
- Thin HTTP wrapper for debugging and manual testing

## Phase 3 – Soul generator

- Curated base souls per domain
- Generation of a request-specific `SOUL.md`
- Integration into `compose_agent` output

## Phase 4 – RAG over skills

- Embedding-based retrieval over skill manifests once the catalog grows past roughly 100 skills
- Ranking and conflict resolution between overlapping skills

## Phase 5 – A2A

- Agent-to-agent protocol support, once the composer and its output format are stable

---

**Current status:** pre-Phase 1. Repository structure and concepts only.
