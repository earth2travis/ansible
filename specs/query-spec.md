---
title: "Query Specification"
date: 2026-04-22
status: "review"
version: "1.0"
---

# Query Specification (`_query.py`)

## Purpose

Execute stored queries against the Substrate to test knowledge coverage, extract synthesized answers, and measure the system's comprehension.

## Query File Format

Each query lives in `research/queries/` as a `.md` file:

```markdown
---
title: "Question description"
category: identity|conflict-resolution|temporal-awareness|synthesis
created: YYYY-MM-DD
last_run: YYYY-MM-DD
score: null
---

# Query: Title

## Question
The actual question being asked.

## Expected Answer Elements
- Bullet list of key facts the correct answer should contain
- Sources that should be cited

## Ground Truth
The authoritative answer (written by a human).
```

## Query Categories

- **identity:** Who/what is something? Tests entity knowledge.
- **conflict-resolution:** When sources disagree, which wins? Tests provenance handling.
- **temporal-awareness:** What changed and when? Tests temporal reasoning.
- **synthesis:** Cross-domain questions requiring combining multiple insights.

## Execution Engine

`_query.py` performs:

1. Load all queries from `research/queries/`
2. Answer each question using only the files in the Substrate (no external knowledge)
3. Cite every file used in the answer
4. Score against the ground truth answer
5. Report results to `evals/YYYY-MM-DD-query-results.md`

## Scoring

Each query scored 0-100:

- **Entity match (30%):** Did it identify the right entities/concepts?
- **Source citation (20%):** Did it cite the authoritative source?
- **Conflict resolution (25%):** If sources disagree, did it pick the right one?
- **Synthesis depth (25%):** Did it combine multiple sources meaningfully?

## Output Format

```json
{
  "run_id": "2026-04-22T03:30:00Z",
  "total_queries": 15,
  "queries_passed": 12,
  "queries_failed": 3,
  "average_score": 78.5,
  "results": [
    {
      "query_file": "research/queries/who-is-taiichi-ohno.md",
      "category": "identity",
      "score": 92,
      "entities_matched": true,
      "sources_correct": true,
      "conflict_resolution": null,
      "synthesis_depth": 85
    }
  ]
}
```

## Usage

```bash
python3 scripts/_query.py                         # Run all queries
python3 scripts/_query.py --category synthesis    # Run only synthesis queries
python3 scripts/_query.py --single 003-foo.md     # Run one query
python3 scripts/_query.py --format json           # Machine-readable output
```

## Integration

Queries run:

1. Weekly as part of the retro process
2. After major ingest runs (to verify knowledge was properly captured)
3. On demand before important decisions requiring Substrate context
