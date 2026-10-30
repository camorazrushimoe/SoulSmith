---
id: agent-task-dispatch
name: Agent task dispatch
description: >
  Hand a task to another crew agent (developer, qa, tech-pm, devops) through
  its webhook door with the crew-send client, using a fixed message format
  that starts with the Linear ticket ID. Use when the PM hands work to another
  agent. Not for publishing status events on the bus or for writing the ticket
  itself.
domain: ops
tags: [multi-agent, dispatch, webhook, handoff, linear]
required_tools: [shell, python3]
optional_tools: [linear]
input_schema:
  agent: "enum(developer|qa|tech-pm|devops)"
  ticket_id: string
  message: string
output_schema:
  dispatch_status: string
compatibility: [hermes-agent, dev-crew]
version: 0.1.0
author: camorazrushimoe
---

# Agent task dispatch

To hand a task to another agent, send a message through their webhook door using the door client.

## From inside a container

```bash
python3 /opt/crew/crew-send.py <agent> "<message>" --container
```

Where `<agent>` is `developer`, `qa`, `tech-pm`, or `devops`.

## Message format

**Always** start with the Linear ticket identifier so completion hooks can bind the turn:

```
Ticket BON-15 — implement /login
Project: <Linear Project name if known>
Goal: ...
Acceptance criteria: ...
Report: comment on the ticket when done with useful detail (the runtime also posts a deterministic completion signal).
```

## Rules

- One task per message.
- Always reference the Linear ticket identifier (`Ticket <ID>`).
- Prefer tickets that are already linked to the effort's Linear Project.
- Track status after dispatch (mark the ticket assigned / in progress).

## Anti-patterns

- Several tasks in one message.
- Messages without the `Ticket <ID>` prefix, which breaks completion hooks.
- Dispatching work that has no ticket in the effort's Linear Project.
- Not updating the ticket status after dispatch.

---

Source: `dev-crew/agents/tech-pm/skills/task-dispatch/SKILL.md`.
