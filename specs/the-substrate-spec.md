---
title: "The Substrate Spec"
date: 2026-04-22
status: "review"
version: "1.1"
---

# The Substrate Spec

A version-controlled shared knowledge graph for the Agent Factory (Zookooree). The Substrate is the nervous system between agents (Koda, Sivart) and Travis (Ξ2T). It follows the Karpathy pattern: raw sources flow through a pipeline into structured knowledge.

## Filesystem Structure

```
substrate/
├── INDEX.md                  # Entry point, nav for the repo
├── SCHEMA.md                 # Frontmatter + naming conventions
├── CONTRIBUTING.md           # Human/agent workflow rules
├── decisions/                # Architecture Decision Records (ADRs)
├── guides/                   # How-to procedures and workflows
├── evals/                    # Output from query/eval script
├── retros/                   # Output from weekly retro script
├── insights/                 # Compiled, cross-referenced knowledge
│   ├── comparisons/          # Head-to-head analysis docs
│   ├── concepts/             # Stable unified concepts
│   └── entities/             # People, orgs, systems, things
├── skills/                   # Shared agent capabilities (future)
├── research/
│   ├── raw/                  # Immutable source material
│   ├── findings/             # Normalized, tagged, linked findings
│   └── queries/              # Stored query definitions
├── scripts/
│   ├── _ingest.py            # Ingest pipeline
│   ├── _lint.py              # Linter
│   ├── _security-scan.py     # Security scanner
│   └── _retro.sh             # Weekly retrospective generator
└── specs/                    # Specifications for system components
    ├── the-substrate-spec.md # This document (master index)
    ├── ingest-spec.md        # _ingest.py behavior and pipeline rules
    ├── lint-spec.md          # _lint.py rules and auto-fix behavior
    ├── query-spec.md         # _query.py format, scoring, execution
    └── security-spec.md      # _security-scan.py checks and patterns
```

## Directory Conventions

| Directory | Purpose | Mutable? |
|---|---|---|
| `research/raw/` | Immutable source material | Append-only |
| `research/findings/` | Normalized, tagged, linked findings | Overwritten by ingest |
| `research/queries/` | Stored query definitions | Append/edit |
| `insights/concepts/` | Stable unified concepts | Manual + auto-promote |
| `insights/entities/` | People, orgs, systems | Manual + auto-promote |
| `insights/comparisons/` | Head-to-head analysis docs | Manual |
| `decisions/` | Architecture Decision Records | Append-only |
| `guides/` | How-to procedures | Append/edit |
| `evals/` | Query/eval script output | Overwritten by evals |
| `retros/` | Weekly retrospective output | Append-only |
| `skills/` | Shared agent capabilities | Manual |
| `specs/` | System component specs | Append/edit |

## Pipeline

```
Capture → Process → Synthesize → Evaluate
raw/   → findings/ → insights/ → evals/
                              ↳ retros/
```

1. **Capture:** Raw sources land in `research/raw/`.
2. **Process:** `_ingest.py` normalizes each raw file into `research/findings/`.
3. **Synthesize:** When a concept appears across 2+ findings, candidate for `insights/`.
4. **Evaluate:** `_query.py` tests knowledge coverage; reports to `evals/`.
5. **Reflect:** `_retro.sh` generates weekly retrospectives to `retros/`.

The pipeline runs on a cron schedule. See [[spec-directory]] for individual script specs.

## Naming Conventions

- **Files:** kebab-case, lowercase only. `my-topic.md`, never `My_Topic.md`
- **Folders:** lowercase, plural. `insights/`, not `Insight/`
- **Wikilinks:** `[[wikilinks]]` for all internal references. Minimum 2 outbound links per page.
- **Tags:** Broad, stable, no synonyms. One standard tag per concept.
- **Dates:** YYYY-MM-DD format in filenames and frontmatter.

## Decisions Registry

| Decision | Date | Status | Summary |
|---|---|---|---|
| Rename brain-two → substrate | 2026-04-22 | accepted | Repo is earth2travis/substrate |
| Eliminate synthesis/ and operations/ | 2026-04-22 | accepted | No additional value, simplify |
| Keep skills/ intentionally empty | 2026-04-22 | accepted | Build when foundation is stable |
| Write specs before scripts | 2026-04-22 | accepted | Align on behavior before coding |
| Weekly retrospectives | 2026-04-22 | accepted | Data for continuous improvement |
| Add retros/ at root | 2026-04-22 | accepted | First-class artifact parallel to evals/ |
| Split specs into individual files | 2026-04-22 | accepted | Master spec is index, each script gets its own spec |

## Related

- [[ingest-spec]]
- [[lint-spec]]
- [[query-spec]]
- [[security-spec]]
- [[context-eval-engine-spec]]

## Open Questions

1. Should the ingest pipeline also process non-markdown sources (PDFs, images with OCR)?
2. What's the threshold for auto-promoting insights vs. requiring manual review?
3. Should the security scan integrate with GitHub's secret scanning API?
4. How should the Substrate handle conflicting information from two authoritative sources?
5. What's the retention policy for raw sources? Infinite, or should stale content be archived?
