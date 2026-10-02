# GitHub Publication Plan

## Recommended repository identity

Suggested name:

`jev-literature-reranking-evaluation`

Suggested description:

> A reproducible exploratory evaluation of TypeSafe JEV for reranking research-paper candidates in a literature-review RAG pipeline.

## Narrative

The public story should be:

1. We encountered a new typed decision model and identified relevance scoring as a plausible use case.
2. We did not immediately change the production architecture.
3. We built a frozen, provenance-bound comparison over 17 reviewed topics and 340 candidate rows.
4. We compared the existing order, pure JEV, and a hybrid that preserved citation rules.
5. Hybrid JEV was best for top-five ranking, nDCG@10, and MRR@10.
6. It did not improve candidate recall or top-ten recall, and RAGAS answer-quality differences were inconclusive.
7. We therefore documented JEV as promising but not ready to replace the current system.

## Repository contents

```text
jev-literature-reranking-evaluation/
├── README.md
├── docs/
│   ├── RESEARCH_REPORT.md
│   ├── METHODOLOGY.md
│   ├── RESULTS.md
│   ├── LIMITATIONS.md
│   ├── REPRODUCIBILITY.md
│   └── LINKEDIN_POST.md
├── evidence/
│   ├── artifact-hashes.csv
│   ├── metrics.csv
│   └── questions.csv
├── scripts/                 # reviewed evaluation scripts
├── tests/                   # deterministic offline tests
├── LICENSE                  # choose deliberately
└── CITATION.cff             # add author and release metadata
```

## Before publishing

- Confirm permission to publish evaluation scripts developed inside the internship/project context.
- Confirm that the project name, employer, mentor, and proprietary architecture may be mentioned.
- Review third-party API and paper-metadata terms.
- Remove absolute local paths, usernames, credentials, and private repository references.
- Run secret scanning over the complete Git history.
- Start the new repository with a clean history rather than copying the original application's `.git` directory.
- Decide whether scripts are released under MIT, Apache-2.0, or kept source-available.
- Add screenshots or charts generated only from aggregate metrics.
- Link the exact release/tag in the LinkedIn post after publication.

## Claims checklist

Safe:

- “Hybrid JEV was the best tested arm for Recall@5 on our 17-topic snapshot.”
- “The result is exploratory and requires a larger study.”
- “JEV reranking was inexpensive and sub-second in this offline run.”

Avoid:

- “JEV is the best reranker.”
- “JEV significantly improves RAG quality.”
- “JEV replaces vector search or retrieval.”
- “The results prove production readiness.”
