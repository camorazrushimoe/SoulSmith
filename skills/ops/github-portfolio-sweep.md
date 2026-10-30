---
id: github-portfolio-sweep
name: GitHub portfolio sweep
description: >
  Scrum Master routine: collect repo activity and open issues/PRs across a
  GitHub org for the last N days, group repos as active/quiet/silent, classify
  issues into buckets (bug, feature, tech-debt, performance, docs, question),
  propose labels for human approval, and update the portfolio memory file. Use
  for a morning or on-request project-status sweep. Read and triage only: not
  for applying labels without approval, merging PRs or editing code.
domain: ops
tags: [github, portfolio, triage, status-report, scrum-master, issues]
required_tools: [python3, github]
optional_tools: [file-edit]
input_schema:
  days: integer
  org: string
output_schema:
  summary: markdown
  proposed_labels: list
  portfolio_update: yaml
compatibility: [hermes-agent, agent-office, github]
version: 0.1.0
author: camorazrushimoe
---

# GitHub portfolio sweep

The Scrum Master's morning routine: sweep GitHub for what's moving, triage the
inbox, and keep the **portfolio** (memory of active projects) in order.

## When to use

- Every morning (or whenever the human asks for a "sweep", "project status",
  or "what is happening").
- When the human wants to know which projects are active and what's in flight.

## How to sweep

1. Run the collector:

   ```bash
   python3 /opt/data/skills/agent-office/scrum-sweep/sweep.py --days 7
   ```

   (or `--json` for structured data). **Do not** prefix it with
   `GITHUB_TOKEN=$GITHUB_TOKEN`: Hermes deliberately strips `GITHUB_TOKEN` /
   `GH_TOKEN` from tool subprocess environments, so that variable is always
   empty for you. The script resolves the token itself from the factory's
   single source of truth (`$TOKENS_FILE`, mounted read-only). If it exits with
   "No GitHub token", the tokens file or `/opt/office-lib` is not mounted —
   report that instead of falling back to the unauthenticated API (60 req/h,
   public repos only), which silently gives an incomplete picture.

   It lists the org's repos grouped by activity — active (pushed in the
   window), quiet (open issues but no push), silent — with each repo's open
   issues and their labels.

2. **Read the portfolio** (memory): `/opt/data/portfolio.yaml`. It records which
   projects you already track, their stage, and their tags.

## Analysis (the value-add)

From the sweep output, produce a short summary answering:

- **Which projects are active** — repos pushed/worked in the last 7 days.
- **What's in flight** — open PRs and the issues being worked.
- **What's stale** — repos with open issues but no recent activity.

## Triage / classification

For repos with a pile of untriaged issues, classify each issue into buckets and
**propose** GitHub labels (do NOT apply them yourself without the human's OK):

| Label | Meaning |
|-------|---------|
| `bug` | something is broken / wrong behaviour |
| `feature` | new functionality / enhancement |
| `tech-debt` | refactor, cleanup, "make it maintainable" |
| `performance` | speed / resource / latency problems |
| `docs` | documentation |
| `question` | needs clarification before any work |

Group and report by bucket, e.g. *"spaced-bro: 6 bugs, 2 performance, 4
tech-debt — suggest labelling and batching the bugs for a fix sprint."*

## Portfolio update

After the sweep, update `/opt/data/portfolio.yaml`:

- Add repos that became active; mark repos that went quiet as `stage: parked`
  (or keep them, with `last_activity` updated).
- Set/refresh `stage` per project: `research | spec | implementation | review |
  done | parked`.
- Keep `tags` reflecting the buckets you triaged.

The portfolio is the durable memory — do not rely on the conversation context
to remember which projects matter.

## Output / summary format

Deliver a compact Telegram-friendly summary to the human (via the external
Hermes agent or directly):

```
🧹 Morning sweep — <date>
🟢 Active: spaced-bro (3 issues · 2 PRs), club27 (…)
🟡 Stalled: board (12 issues)
Triage: spaced-bro → 6 bug / 2 perf / 4 tech-debt. Suggest adding labels
and sending the 6 bugs to dev-crew as one batch.
```

## Rules

- Labels are **proposed first, applied after the human approves**.
- The portfolio is updated every sweep — it's the source of "what's active".
- Consult other agents (architect, staff-engineer) if an issue's nature is
  unclear, but you can decide on your own.
- Never push to `main` / merge PRs while sweeping — this skill is read + triage
  only. Code changes go through the normal PR flow (`docs/github-workflow.md`).

## Anti-patterns

- Applying labels before the human approves them.
- Falling back to the unauthenticated GitHub API and reporting an incomplete picture as complete.
- Relying on conversation memory instead of updating the portfolio file.
- Pushing, merging or changing code during a sweep.

## Appendix: sweep.py

Original file `sweep.py`.

```python
#!/usr/bin/env python3
"""Scrum Master — GitHub sweep.

Collects the org's repos, open issues, open PRs and recent activity (last N
days) so the Scrum Master can see what's active and triage work.

Usage:
  GITHUB_TOKEN=<token> python3 sweep.py            # readable summary
  GITHUB_TOKEN=<token> python3 sweep.py --json     # machine-readable
  GITHUB_TOKEN=<token> python3 sweep.py --days 14  # wider window

Stdlib only. Handles rate-limit (403/429) with backoff, follows pagination
(Link header), and never lets one repo's failure kill the whole sweep.
"""
from __future__ import annotations

import argparse
import json
import os
import re
import sys
import time
import urllib.error
import urllib.request
from datetime import datetime, timezone, timedelta

ORG = "camorazrushimoe"
API = "https://api.github.com"


def _retry_after(e: urllib.error.HTTPError) -> float | None:
    v = e.headers.get("Retry-After") if e.headers else None
    if v is None:
        return None
    try:
        return float(v)
    except ValueError:
        return None


def gh_all(url: str, token: str, retries: int = 4):
    """GET with rate-limit backoff + Link-header pagination. Returns a list."""
    items: list = []
    while url:
        for attempt in range(retries):
            req = urllib.request.Request(
                url,
                headers={
                    "Authorization": f"Bearer {token}",
                    "Accept": "application/vnd.github+json",
                    "X-GitHub-Api-Version": "2022-11-28",
                },
            )
            try:
                with urllib.request.urlopen(req, timeout=30) as r:
                    remaining = r.headers.get("X-RateLimit-Remaining")
                    if remaining is not None and remaining.isdigit() and int(remaining) < 10:
                        print(f"[warn] GitHub rate limit low: {remaining} remaining", file=sys.stderr)
                    data = json.load(r)
                    link = r.headers.get("Link", "")
                if isinstance(data, list):
                    items.extend(data)
                else:
                    items.append(data)
                    return items
                nxt = None
                for part in link.split(","):
                    if 'rel="next"' in part:
                        m = re.search(r"<([^>]+)>", part)
                        nxt = m.group(1) if m else None
                url = nxt if nxt else ""
                break  # success, move to next page
            except urllib.error.HTTPError as e:
                if e.code in (403, 429) and attempt < retries - 1:
                    wait = _retry_after(e) or (2 ** attempt)
                    print(f"[warn] rate limited ({e.code}), retrying in {wait:.0f}s", file=sys.stderr)
                    time.sleep(wait)
                    continue
                raise
        else:
            raise RuntimeError(f"rate limit exhausted for {url}")
    return items


def parse_dt(s: str | None) -> datetime | None:
    if not s:
        return None
    return datetime.fromisoformat(s.replace("Z", "+00:00"))


def time_ago(dt: datetime | None, now: datetime) -> str:
    if dt is None:
        return "—"
    secs = max(0, (now - dt).total_seconds())
    if secs < 3600:
        return f"{int(secs // 60)}m ago"
    if secs < 86400:
        return f"{int(secs // 3600)}h ago"
    return f"{int(secs // 86400)}d ago"


def collect(token: str, org: str, days: int) -> list[dict]:
    cutoff = datetime.now(timezone.utc) - timedelta(days=days)
    now = datetime.now(timezone.utc)

    try:
        repos = gh_all(f"{API}/users/{org}/repos?per_page=100&type=owner", token)
    except Exception as e:
        print(f"[error] repo list failed: {e}", file=sys.stderr)
        sys.exit(1)

    result = []
    for r in repos:
        if r.get("fork"):
            continue
        name = r["name"]
        pushed_dt = parse_dt(r.get("pushed_at"))
        active = bool(pushed_dt and pushed_dt >= cutoff)

        try:
            items = gh_all(f"{API}/repos/{org}/{name}/issues?state=open&per_page=50", token)
        except Exception as e:
            print(f"[warn] issues for {name} failed ({e}); skipping", file=sys.stderr)
            items = []

        issues = [i for i in items if "pull_request" not in i]
        prs = [i for i in items if "pull_request" in i]
        result.append({
            "repo": name,
            "active": active,
            "pushed_at": r.get("pushed_at"),
            "open_issues": len(issues),
            "open_prs": len(prs),
            "issues": [
                {
                    "number": i["number"],
                    "title": i["title"],
                    "labels": [l["name"] for l in i.get("labels", [])],
                    "created_at": i.get("created_at"),
                    "updated_at": i.get("updated_at"),
                }
                for i in issues
            ],
        })
    result.sort(key=lambda x: (not x["active"], -x["open_issues"], -x["open_prs"]))
    return result


def render(data: list[dict], days: int) -> str:
    now = datetime.now(timezone.utc)
    lines = [f"🧹 **GitHub sweep** — {now.strftime('%d %b %H:%M UTC')} (last {days}d)", ""]
    active = [d for d in data if d["active"]]
    quiet = [d for d in data if not d["active"] and (d["open_issues"] or d["open_prs"])]
    silent = [d for d in data if not d["active"] and not d["open_issues"] and not d["open_prs"]]

    lines.append(f"🟢 **Active** ({len(active)})")
    for d in active:
        lines.append(
            f"▸ `{d['repo']}` — {d['open_issues']} issues, {d['open_prs']} PRs · pushed {time_ago(parse_dt(d['pushed_at']), now)}"
        )
        for i in d["issues"]:
            labs = f" [`{'`,`'.join(i['labels'])}`]" if i["labels"] else ""
            lines.append(f"    #{i['number']} {i['title'][:70]}{labs}")
    if active:
        lines.append("")

    lines.append(f"🟡 **Quiet but open** ({len(quiet)})")
    for d in quiet:
        lines.append(f"▸ `{d['repo']}` — {d['open_issues']} issues, {d['open_prs']} PRs")
    if quiet:
        lines.append("")

    lines.append(f"⚪ **Silent** ({len(silent)}): " + ", ".join(f"`{d['repo']}`" for d in silent))
    return "\n".join(lines).rstrip() + "\n"


def resolve_token() -> str:
    """Find a GitHub token, tolerating Hermes' subprocess hardening.

    Hermes strips GITHUB_TOKEN / GH_TOKEN from tool subprocess environments
    (tools/environments/local.py::_ALWAYS_STRIP_KEYS), so an agent running
    this script can never inherit it — the env var only works when a human
    runs the script on the host. Inside a factory container the token comes
    from the single source of truth mounted at $TOKENS_FILE.
    """
    for var in ("GITHUB_TOKEN", "GH_TOKEN"):
        val = os.environ.get(var, "").strip()
        if val:
            return val

    tokens_file = os.environ.get("TOKENS_FILE", "/opt/tokens/tokens.yaml")
    if not os.path.isfile(tokens_file):
        return ""
    for lib in ("/opt/office-lib", os.path.join(os.path.dirname(
            os.path.abspath(__file__)), "..", "..", "..", "..", "office")):
        lib = os.path.abspath(lib)
        if os.path.isdir(lib) and lib not in sys.path:
            sys.path.insert(0, lib)
    try:
        from credentials import load_tokens  # noqa: PLC0415
        return ((load_tokens(tokens_file).get("github") or {})
                .get("token") or "").strip()
    except Exception:
        return ""


def main() -> None:
    ap = argparse.ArgumentParser()
    ap.add_argument("--days", type=int, default=7)
    ap.add_argument("--org", default=ORG)
    ap.add_argument("--json", action="store_true")
    args = ap.parse_args()

    token = resolve_token()
    if not token:
        sys.exit("No GitHub token: set GITHUB_TOKEN, or make sure "
                 "$TOKENS_FILE (default /opt/tokens/tokens.yaml) has "
                 "github.token and /opt/office-lib is mounted.")

    data = collect(token, args.org, args.days)
    if args.json:
        print(json.dumps(data, indent=2, ensure_ascii=False))
    else:
        print(render(data, args.days))


if __name__ == "__main__":
    main()
```

Note: the script hardcodes `ORG = "camorazrushimoe"`; change it for other orgs.

---

Source: `agent-office/agents/scrum-master/skills/scrum-sweep/ (SKILL.md, sweep.py); Russian trigger phrases and summary template translated to English`.
