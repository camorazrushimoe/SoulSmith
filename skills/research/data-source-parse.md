---
id: data-source-parse
name: Quick research parser
description: >
  Build lightweight, disposable parsers and collectors for research data
  sources (web pages, APIs, public datasets, files), respecting rate limits
  and terms. Use when an experiment needs data from a source no existing tool
  reads. Not for production scrapers or long-running pipelines.
domain: research
tags: [parsing, scraping, data-collection, apis, research]
required_tools: [python3]
optional_tools: [web-fetch, shell]
input_schema:
  source: string
  fields: list
output_schema:
  parser_script: string
  sample_output: string
compatibility: [lab-crew, hermes-agent]
version: 0.1.0
author: Sergey Ryabushko
---

# Quick research parser

## Purpose

Build lightweight parsers and collectors for research data sources (web pages, APIs, public datasets, files).

## When to use

- An experiment needs data that is not already available locally
- The goal is signal-finding, not production-grade ingestion

## Principles

1. **Minimum viable collection** — collect only fields required by the hypothesis
2. **Temporary by default** — store under `workspace/<topic>/data/`
3. **Readable scripts** — prefer clear Python scripts over clever frameworks
4. **Document limitations** — rate limits, missing fields, sampling bias, ToS risks
5. **Stop early** — if the first small sample already answers the question, do not scale yet

## Typical sources (examples)

- Public product pages / store pages (e.g. Steam-like)
- Review / comment pages
- Simple public APIs
- CSV / JSON dumps
- Search result pages (with care)

## Anti-patterns

- Building a general-purpose crawler "for later"
- Persisting to production databases during research
- Ignoring robots.txt / ToS / rate limits when it creates risk
- Collecting everything "just in case"

---

Source: `lab-crew/skills/quick-parsing/SKILL.md`.
