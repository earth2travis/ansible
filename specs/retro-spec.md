---
title: "Weekly Retrospective Specification"
date: 2026-04-22
status: "review"
version: "1.0"
---

# Weekly Retrospective Specification (`_retro.sh`)

## Purpose

The retrospective script analyzes the Substrate's own health over the past week. It produces a structured report that drives the continuous improvement loop. The retro is not a daily log or a summary — it's a weekly diagnostic that identifies gaps and generates action items.

## What It Reads

The script pulls data from multiple sources:

1. **Git log** — New commits, deletions, and renames from the past 7 days (`git log --since="7 days ago" --oneline --name-status`)
2. **Filesystem state** — Current file counts per directory, new orphans, modified files
3. **Ingest output** — Structured log from `_ingest.py` (if it ran this week)
4. **Eval reports** — Latest eval scores from `evals/` (if eval ran this week)
5. **Open questions** — The "Open Questions" section of the substrate spec

## What It Writes

Output is a single file: `retros/week-YYYY-WNN.md` (ISO week number, e.g., `week-2026-W17.md`).

Retros are append-only and immutable. Each file stands alone — it does not reference or modify prior retros.

## Report Structure

```markdown
---
title: "Retrospective: Week YYYY-WNN"
date: YYYY-MM-DD
week: "YYYY-WNN"
---

# Retro: Week YYYY-WNN

## What Changed
- Files added: N
- Files removed: N
- Files modified: N
- New raw sources: [list]
- New insights promoted: [list]
- New decisions recorded: [list]
- New specs added/updated: [list]

## Metrics
- `research/raw/`: N files
- `research/findings/`: N files
- `insights/concepts/`: N files
- `insights/entities/`: N files
- `insights/comparisons/`: N files
- `decisions/`: N files
- `guides/`: N files
- `evals/`: N reports
- `specs/`: N specs

## Patterns
- Emerging themes this week
- Topics that appeared across multiple sources
- Notable cross-reference density changes

## Gaps
### Structural
- Files without frontmatter
- Broken wikilinks
- Orphan findings (findings without raw sources)

### Knowledge
- Topics with raw sources but no findings
- Topics with findings but no insights
- Low-coverage query categories
- Stale content (no `last_confirmed` update in N weeks)

### Process
- Scripts that ran with errors
- Eval scores trending down
- Open questions that remain unresolved

## Actions
1. [Action item with owner]
2. [Action item with owner]
3. [Action item with owner]
```

## Implementation: `_retro.sh`

```bash
#!/bin/bash
# _retro.sh — Generate weekly retrospective for the Substrate
# Usage: ./_retro.sh [--repo /path/to/substrate] [--dry-run]

REPO="${REPO:-.}"
DRY_RUN=false
WEEK_NUM=$(date +%G-W%V)

# 1. Filesystem metrics per directory
# 2. Git log diff from 7 days ago
# 3. Ingest log summary (if exists)
# 4. Latest eval report (if exists)
# 5. Assemble report template
# 6. Write to retros/week-$WEEK_NUM.md

# The script uses only stdlib tools: git, find, wc, date, cat
# No external dependencies.
```

## Cron Integration

Scheduled for Sunday 05:00 UTC (after eval completes at 04:00 UTC). Runs via cron:

```cron
0 5 * * 0 cd /path/to/substrate && bash scripts/_retro.sh
```

## Usage

```bash
cd /path/to/substrate
bash scripts/_retro.sh                          # Run for current week
bash scripts/_retro.sh --repo /path/to/substrate  # Custom repo path
bash scripts/_retro.sh --dry-run                 # Print report to stdout without writing
```
