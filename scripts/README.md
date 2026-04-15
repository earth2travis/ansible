# `scripts/_ingest.py` — Knowledge Ingestion Pipeline

Reads raw sources from `research/raw/`, normalizes frontmatter to SCHEMA,
auto-tags, discovers cross-references, writes findings, and promotes
topics to `insights/` when patterns emerge across 2+ findings.

## Usage

```
python3 scripts/_ingest.py [--dry-run] [--repo /path/to/ansible]
```

Run against the repo root. `--dry-run` previews output without writing.

## Pipeline

```
research/raw/  (immutable sources, flat, kebab-case)
       ↓
  _ingest.py normalizes + discovers relationships
       ↓
research/findings/  (SCHEMA-compliant, every raw source normalized)
       ↓
  topics mentioned in 2+ findings get promoted
       ↓
insights/entities/   (people, companies, platforms)
insights/concepts/   (patterns, systems, frameworks)
```

## Output Schema

Every file in `findings/` and `insights/` has frontmatter:

```yaml
---
title: "Page Title"
tags: [kebab-case-tag]
related: [[other-file-stem]], [[another-file]]
source: research/raw/original-source.md
---
```

## Naming Rules

- **Kebab-case with hyphens only** — underscores are normalized
- Date prefix uses hyphens: `2026-04-08-title.md`
- No noise suffixes — content type lives in frontmatter, not the filename
- `normalize_filename()` converts all output to the same convention
- When two raw sources cover overlapping topics, semantic suffixes
  distinguish them (e.g., `-report`, `-study`, `-coding`)

## Implementation

- Python 3, stdlib only (no pip dependencies)
- Minimal YAML parser handles our frontmatter shapes
- Auto-tagging via keyword scanning against known patterns
- Cross-referencing via wikilinks in body + shared tags + title mentions
- HTML scrapes (`<!DOCTYPE`) detected and skipped
- Idempotent — rerunning overwrites findings, adds new insights only

## Raw File Policy

Raw files in `research/raw/` are never modified by this script.
Filenames *can* be renamed (naming fixes, not content changes), but the
script treats raw as read-only source material.
