---
title: "Ingest Pipeline Specification"
date: 2026-04-22
status: "review"
version: "1.0"
---

# Ingest Pipeline Specification (`_ingest.py`)

## Purpose

Transform raw, heterogeneous source material into normalized, cross-referenced knowledge files. The ingest pipeline is the heart of the Substrate.

## Current Behavior

The existing `scripts/_ingest.py` already implements:

1. Scan `research/raw/*.md`
2. Parse YAML frontmatter (or infer if missing)
3. Normalize to SCHEMA: title, tags, related, source
4. Auto-tag using strict phrase matching (no single-word keywords)
5. Cross-reference via wikilink extraction + title mention frequency
6. Write normalized findings to `research/findings/`
7. Promote to `insights/` when a topic appears in 2+ findings
8. Orphan cleanup: remove findings/ for deleted raw sources

## Required Behavior

**Must have:**

- Proper handling of `research/queries/`, `decisions/`, `guides/`, `retros/`, `specs/` as separate directories — not touched by ingest
- Explicit logging to a structured log file (currently logs to stdout only)
- Idempotency: running ingest twice produces the same output without duplicates
- Respect `no-ingest` YAML frontmatter flag (skip files marked for manual handling)

**Should have:**

- Incremental mode: only process new/changed raw files since last run
- Tag conflict resolution: when a file manually has tags, don't auto-add conflicting ones
- Related link quality score: weight wikilinks higher than title mentions
- Summary output: what was added, changed, removed since last run

**Nice to have:**

- Content deduplication detection: flag near-duplicate raw files
- Source freshness scoring: prefer newer sources when conflicts exist

## Output Format

Ingest produces a structured log:

```json
{
  "run_id": "2026-04-22T03:30:00Z",
  "raw_scanned": 300,
  "html_excluded": 3,
  "findings_written": 297,
  "findings_updated": 12,
  "findings_removed": 2,
  "insights_promoted": 4,
  "insights_already_existed": 15,
  "orphans_cleaned": 2,
  "new_raw_since_last_run": 5,
  "errors": []
}
```

## Usage

```bash
python3 scripts/_ingest.py                          # Full run
python3 scripts/_ingest.py --repo /path/to/substrate  # Custom repo path
python3 scripts/_ingest.py --dry-run                # Preview without writing
python3 scripts/_ingest.py --incremental            # Only new/changed files
python3 scripts/_ingest.py --log-file /path/to/log  # Structured output
```
