# Limitations and Next Experiment

## Current limitations

1. **Small sample:** only 17 topics were tested.
2. **Frozen candidate pool:** JEV reordered papers but could not retrieve missing papers.
3. **Binary labels:** relevance was not graded by degree.
4. **Topic sensitivity:** Answer Relevancy was strongly influenced by `halluc-01`.
5. **Reference-free answer evaluation:** 15 topics lacked reviewed reference answers, so Context Precision and Context Recall were excluded.
6. **Single saved answer run:** LLM generation and judge behavior can vary over time.
7. **Multiple comparisons:** several related metrics and two reranking arms were explored.
8. **No production traffic:** latency, reliability, and cost were measured offline.
9. **No comparison with established rerankers:** the experiment compared JEV with the current ordering, not Cohere, cross-encoders, ColBERT, or another dedicated reranker.
10. **Early-access model:** model behavior, pricing, and API contracts may change.

## Why JEV should not replace the full system

JEV is a decision model used here for relevance scoring. It does not replace:

- search-provider retrieval;
- embeddings and candidate generation;
- citation-protection policy;
- answer generation;
- reference validation;
- application-level authorization, persistence, and observability.

The experiment suggests a possible role for JEV inside a larger pipeline, not a complete replacement architecture.

## Proposed next experiment

Use at least 100 diverse, independently labelled topics and preregister:

- Primary outcome: Recall@5.
- Safety outcome: Recall@10 must not regress beyond an approved margin.
- Secondary outcomes: Precision@5, nDCG@10, MRR@10, latency, and cost.
- Baselines: current order, hybrid JEV, and at least one established reranker.
- Deeper candidate pools with identical membership across arms.
- Repeated answer/judge runs or an independent human evaluation subset.
- Reviewed references sufficient for Context Precision and Context Recall.

A reasonable non-regression rule is that the lower paired 95% confidence bound for Recall@10 difference must remain above -0.02. This threshold is only a planning example and must be approved before data collection.

## Production gate

Do not integrate JEV into production until the larger study demonstrates:

1. Repeatable Recall@5 improvement.
2. Recall@10 non-regression.
3. Stable gains across topic families.
4. Bounded latency and failure behavior under concurrency.
5. A safe fallback to the current ranking.
