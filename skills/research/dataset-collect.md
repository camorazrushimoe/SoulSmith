---
id: dataset-collect
name: Experiment dataset collection
description: >
  Collect and organize temporary datasets needed for an experiment, with
  provenance and a README, without building production data infrastructure.
  Use when an experiment needs data assembled from existing sources. Not for
  writing parsers or scrapers for a new source (use data-source-parse) or for
  production pipelines.
domain: research
tags: [datasets, data-collection, provenance, experiments]
required_tools: [shell, file-write]
optional_tools: [python3, web-fetch]
input_schema:
  data_needs: string
output_schema:
  dataset_path: string
  provenance_notes: markdown
compatibility: [lab-crew, hermes-agent]
version: 0.1.0
author: Sergey Ryabushko
---

# Experiment dataset collection

## Purpose

Collect and organize temporary datasets needed for experiments — without building production infrastructure.

## When to use

- An experiment design requires data that is not yet local
- Multiple sources must be combined for a single test

## Principles

1. Collect only what the current hypothesis needs
2. Store under `workspace/<topic>/data/`
3. Prefer flat, inspectable formats (CSV, JSONL, parquet) over opaque stores
4. Record provenance: source, date collected, sampling method, known gaps
5. Keep a short `data/README.md` describing what is in the folder

## Minimum documentation for a dataset

```markdown
# Dataset: <name>
- Source: ...
- Collected: YYYY-MM-DD
- Sample size: ...
- Fields: ...
- Known limitations: ...
```

## Anti-patterns

- Silent collection with no provenance notes
- Dumping everything into one undocumented folder
- Treating temporary research data as a long-lived product asset

---

Source: `lab-crew/skills/data-collection/SKILL.md`.
