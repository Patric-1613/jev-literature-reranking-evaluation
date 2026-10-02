# Evidence and Reproducibility

## Frozen evidence

The experiment is bound to cryptographic hashes listed in [`evidence/artifact-hashes.csv`](../evidence/artifact-hashes.csv). A rerun should refuse any artifact whose bytes do not match the reviewed hash.

## Evidence chain

1. Freeze 17 reviewed topics and their relevance annotations.
2. Capture at least 20 candidates per topic.
3. Validate and hash the candidate snapshot.
4. Run one pinned JEV scoring call per topic.
5. Validate response-to-snapshot binding.
6. Reconstruct baseline, pure-JEV, and hybrid rankings deterministically.
7. Compute paired retrieval metrics.
8. Freeze reviewed answer-evaluation questions and top-five contexts.
9. Generate paired baseline/hybrid answers under identical settings.
10. Compute reference-free RAGAS metrics.
11. Recompute statistics from saved artifacts and record final hashes.

## Public evidence policy

Recommended for the public repository:

- methodology and final report;
- scripts and deterministic tests;
- aggregate and topic-level metric tables;
- artifact hashes and schemas;
- sanitized execution metadata;
- model names, limits, and pricing assumptions.

Review before publishing:

- candidate titles and identifiers;
- question/reference material originating from earlier evaluation datasets;
- provider response payloads;
- paper abstracts governed by third-party licences or API terms.

Never publish:

- API keys or authorization headers;
- `.env` files;
- raw error traces containing request data;
- private database contents;
- unrelated application data.

## Independent reproduction

An independent reproduction should use its own provider credentials and create new raw artifacts. It should not treat the hashes in this repository as downloadable substitutes for source data. The hashes demonstrate which private artifacts produced the published result.

## External references

- TypeSafe AI, [Introducing System One Models & JEV](https://typesafe.ai/blog/introducing-system-one-models-and-jev). Vendor description of JEV's intended interface, speed, and pricing; vendor claims were not treated as experimental results.
- RAGAS, [Faithfulness](https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/faithfulness/).
- RAGAS, [Answer Relevancy](https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/answer_relevance/).
