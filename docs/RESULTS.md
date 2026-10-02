# Results

## Retrieval comparison

| Macro metric | Baseline | Pure JEV | Hybrid JEV |
|---|---:|---:|---:|
| Candidate-pool recall | 0.6078 | 0.6078 | 0.6078 |
| Precision@5 | 0.0235 | 0.0941 | **0.1647** |
| Precision@10 | **0.1000** | 0.0824 | 0.0941 |
| Recall@5 | 0.1176 | 0.3039 | **0.5000** |
| Recall@10 | **0.6078** | 0.5294 | 0.5784 |
| nDCG@10 | 0.2712 | 0.3057 | **0.4111** |
| MRR@10 | 0.1662 | 0.2678 | **0.4085** |

## Where hybrid JEV was best

- **Top-five precision:** roughly seven times the baseline point estimate.
- **Top-five recall:** over four times the baseline point estimate.
- **MRR@10:** relevant papers appeared earlier.
- **nDCG@10:** the overall top-ten order was better even though top-ten recall declined.
- **Recorded JEV efficiency:** 17 calls cost approximately $0.00523, with p50 latency around 312 ms.

The strongest practical conclusion is that JEV helped prioritize papers users would see first.

## Where JEV was weaker

- Baseline retained the best Precision@10 and Recall@10.
- Pure JEV was weaker than hybrid JEV on the main early-ranking metrics.
- Candidate-pool recall did not change, proving that reranking did not solve missing-paper retrieval.
- After adjustment across the related retrieval contrasts, results were not confirmatory.

## Paired answer quality

| Metric | Baseline | Hybrid JEV | Difference | 95% CI | W/T/L |
|---|---:|---:|---:|---:|---:|
| Faithfulness | 0.9213 | 0.9539 | +0.0326 | [-0.0375, 0.1182] | 5/6/6 |
| Answer Relevancy | 0.7928 | 0.8422 | +0.0494 | [-0.0207, 0.1566] | 8/2/7 |

Neither interval excludes zero. Topic-level wins and losses are also balanced, so the answer-quality results should not be presented as a reliable improvement.

### Sensitivity check

`halluc-01` contributed a +0.7985 Answer Relevancy difference. Removing it reduced the overall mean difference from +0.04945 to +0.00263. This is the clearest warning against overselling the answer-quality result.

## Statistical interpretation

Hybrid Recall@5 showed the strongest nominal paired signal, but the study tested multiple correlated metrics and two reranking arms. After Holm adjustment across 14 retrieval contrasts, the smallest adjusted values were approximately 0.0547. The correct interpretation is **promising exploratory evidence**, not confirmation.

## Cost separation

| Stage | Purpose | Estimated cost |
|---|---|---:|
| Candidate capture | Build frozen evidence | $0.00849 OpenAI cost |
| JEV scoring | Test reranking | $0.00523 |
| Full paired RAGAS | Evaluate generated answers | $0.40863 |

The RAGAS cost is evaluation expense and should not be described as JEV serving cost.
