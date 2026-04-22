---
title: "Query Specification"
date: 2026-04-22
status: "review"
version: "2.0"
---

# Query Specification (`_query.py`)

## Purpose

Queries are the stored Q&A interface to the Substrate. They allow agents and humans to ask complex questions and receive synthesized answers drawn only from Substrate files. Queries are the **primary way knowledge is consumed**.

## Query vs. Eval

| | Queries | Evals |
|---|---|---|
| **Audience** | Agents and humans using the Substrate | System maintainers testing the Substrate |
| **Purpose** | Retrieve knowledge for current work | Verify the Substrate is working correctly |
| **Ground truth** | No ground truth -- answers come from Substrate | Ground truth answers for comparison scoring |
| **Output** | Answer returned to caller | Scored report saved to `evals/` |
| **Frequency** | Ad-hoc, on-demand | Scheduled (weekly) |
| **Scoring** | No scoring | Scored 0-100 via SAS metric |

Queries are how you **use** the Substrate. Evals are how you **test** it.

## Query File Format

Each query lives in `research/queries/` as a `.md` file:

```markdown
---
title: "Question description"
category: identity|state|conflict|synthesis
created: YYYY-MM-DD
---

# Query: Title

## Question
The question being asked. Written as a natural-language query.

## Expected Format
Describe the structure of the expected answer:
- What entities should be identified
- What decisions or facts should be cited
- What relationships should be surfaced
```

Key differences from eval questions:
- No `last_run` or `score` fields (queries are not scored)
- No `Ground Truth` section (answers come from the Substrate, not pre-written)
- `Expected Format` describes what a good answer looks like, not what the answer is

## Query Categories

- **identity** — "Who is X?", "What is the role of Y?", "What does Z mean?"
  Retrieval of entity and concept definitions.

- **state** — "What are we working on?", "What decisions have been made?",
  "What's our current stance on X?"
  Retrieval of current organizational state from decisions, insights, and recent findings.

- **conflict** — "Which source is authoritative on X?", "What do sources disagree on?"
  Identification of conflicting information and citation of the authoritative source.

- **synthesis** — "How do X and Y relate?", "What patterns appear across A, B, and C?"
  Cross-domain answers requiring combining multiple insights and findings.

## Query Engine

`_query.py` processes queries at runtime:

1. Load the query definition from `research/queries/<query>.md`
2. Parse the question and expected format
3. Search the Substrate (findings + insights + decisions) for relevant files
4. Synthesize an answer using only Substrate content
5. Return the answer with source citations
6. Log the query execution (not saved to disk; consumed in real-time)

## Usage

```bash
python3 scripts/_query.py --run "query-filename.md"     # Run one query
python3 scripts/_query.py --run-all                     # Run all queries
python3 scripts/_query.py --category state              # Run all queries in category
python3 scripts/_query.py --run "query.md" --format json # Machine-readable answer
```

## Entity Expansion

When `_query.py` processes a query, it loads `insights/entities/entity-map.json` and expands any known aliases in the query text. Searching for "Travis" automatically searches for all mapped aliases (`earth2travis`, `@travis`, `Ξ2T`, `travis@synthweave.xyz`). This ensures complete answers across all sources, not just the ones using the exact alias from the question.

## Integration

Queries are invoked on-demand by agents during research, decision-making, or session execution. They are not scheduled -- they run when knowledge is needed.

When an agent is making a decision or writing a spec, it runs queries against the Substrate to ensure its work is grounded in existing knowledge.
