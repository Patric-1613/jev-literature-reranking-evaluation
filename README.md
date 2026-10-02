# Evaluating JEV as a Reranker for Literature Retrieval

## Summary

This repository documents a controlled, offline experiment asking one practical question:

> Can TypeSafe AI's JEV improve the ordering of research papers returned by a literature-retrieval system without changing which papers were retrieved?

We compared three ranking strategies on the same frozen evidence:

1. **Current baseline**: the existing production-equivalent ordering.
2. **Pure JEV**: all candidates reordered by JEV relevance scores.
3. **Citation-partition + JEV**: existing citation-protection rules retained, with JEV used to rank papers inside the protected block and the remaining candidate pool.

The experiment used **17 human-reviewed research questions**, **20 candidates per question**, and **340 frozen candidate rows**. Every arm ranked the same candidates, so changes came from ordering rather than additional retrieval.

## Main finding

The hybrid JEV approach was the **best of the three tested arms for early ranking quality**:

| Metric | Baseline | Pure JEV | Hybrid JEV | Best tested arm |
|---|---:|---:|---:|---|
| Precision@5 | 0.0235 | 0.0941 | **0.1647** | Hybrid JEV |
| Recall@5 | 0.1176 | 0.3039 | **0.5000** | Hybrid JEV |
| nDCG@10 | 0.2712 | 0.3057 | **0.4111** | Hybrid JEV |
| MRR@10 | 0.1662 | 0.2678 | **0.4085** | Hybrid JEV |

This means the hybrid approach was substantially better at moving useful papers toward the first five positions in this particular dataset.

## Where JEV did not win

JEV was not universally better:

| Metric | Baseline | Pure JEV | Hybrid JEV | Observation |
|---|---:|---:|---:|---|
| Precision@10 | **0.1000** | 0.0824 | 0.0941 | Baseline remained best |
| Recall@10 | **0.6078** | 0.5294 | 0.5784 | Both reranked arms declined |
| Candidate-pool recall | 0.6078 | 0.6078 | 0.6078 | Reranking cannot add missing papers |

Reference-free RAGAS evaluation also showed only small, statistically inconclusive answer-quality changes:

| Metric | Baseline | Hybrid JEV | Difference | Interpretation |
|---|---:|---:|---:|---|
| Faithfulness | 0.9213 | 0.9539 | +0.0326 | Positive direction, not significant |
| Answer Relevancy | 0.7928 | 0.8422 | +0.0494 | Positive direction, not significant |

Nearly all practical magnitude in Answer Relevancy came from one topic. Removing `halluc-01` reduced the mean difference from **+0.04945** to **+0.00263**.

## Decision

**Hybrid JEV is promising as a top-five reranker, but this experiment does not justify replacing the current ranking system or enabling JEV in production.**

The appropriate next step is a larger preregistered evaluation with at least 100 independently labelled topics and a Recall@10 non-regression requirement.

## Why JEV was tested

TypeSafe describes JEV as a System One model designed for fast, typed probabilistic decisions rather than open-ended text generation. That makes relevance scoring a plausible use case. Vendor descriptions are background, not evidence for our result; the conclusions here come from the frozen experiment. See the [official TypeSafe introduction](https://typesafe.ai/blog/introducing-system-one-models-and-jev).

For answer-quality checks, we used RAGAS Faithfulness and Answer Relevancy. RAGAS defines Faithfulness as support of answer claims by retrieved context, while Answer Relevancy measures alignment between the response and the user question. See the official [Faithfulness](https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/faithfulness/) and [Answer Relevancy](https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/answer_relevance/) documentation.

## Repository guide

- [Research narrative](docs/RESEARCH_REPORT.md)
- [Methodology and 17-question baseline](docs/METHODOLOGY.md)
- [Results and interpretation](docs/RESULTS.md)
- [Limitations and next experiment](docs/LIMITATIONS.md)
- [Evidence and reproducibility](docs/REPRODUCIBILITY.md)
- [GitHub publication plan](docs/PUBLICATION_PLAN.md)
- [LinkedIn post draft](docs/LINKEDIN_POST.md)
- [Machine-readable metric summary](evidence/metrics.csv)
- [Frozen artifact hashes](evidence/artifact-hashes.csv)
- [Question inventory](evidence/questions.csv)

## Scope

This was an exploratory offline evaluation, not a production A/B test. JEV was never integrated into the application. Raw third-party paper abstracts, API credentials, provider responses, and private runtime artifacts are not included in this publication package.
