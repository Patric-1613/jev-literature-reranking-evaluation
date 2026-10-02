# Research Report

## 1. Research question

The existing literature-review agent already retrieves and ranks papers. We wanted to test whether JEV could act as a second-stage relevance judge and move better papers nearer the top without redesigning the production architecture.

The experiment did not ask whether JEV could replace retrieval, generate answers, or discover papers by itself. It asked whether JEV could improve **ranking within a fixed candidate pool**.

## 2. Hypothesis

Our working hypothesis was:

> If JEV can make useful relevance decisions over a frozen candidate set, then papers labelled relevant should appear more often and earlier in the first five or ten results.

We expected the strongest effect in top-five metrics because reranking is intended to improve what a user sees first.

## 3. Grounded baseline

The comparison used 17 reviewed research questions and 20 candidates per question. Candidate membership was identical across all ranking arms. This prevents a reranker from receiving credit for retrieving additional papers.

The baseline was not a synthetic random ordering. It was the frozen production-equivalent citation-partition ordering from the literature-review system. Relevance labels and stable paper identifiers were frozen before JEV scoring. Artifact hashes bind the candidate snapshot, annotations, JEV responses, comparison, reviewed questions, and paired RAGAS output.

Three arms were compared:

| Arm | Behavior |
|---|---|
| Current baseline | Existing production-equivalent order |
| Pure JEV | All 20 candidates sorted by JEV relevance score |
| Citation-partition + JEV | Existing protected citation block retained; JEV ranks within the protected block and the remaining candidates |

The hybrid arm tests a realistic engineering approach: use JEV where it helps while preserving an existing rule that protects citation-grounded evidence.

## 4. Retrieval findings

Hybrid JEV delivered the strongest early ranking in the experiment:

- Recall@5 increased from **0.1176 to 0.5000**.
- Precision@5 increased from **0.0235 to 0.1647**.
- MRR@10 increased from **0.1662 to 0.4085**.
- nDCG@10 increased from **0.2712 to 0.4111**.

These results show that hybrid JEV was better at moving labelled papers into high-visibility positions.

The result was not uniformly positive:

- Recall@10 declined from **0.6078 to 0.5784**.
- Precision@10 declined from **0.1000 to 0.0941**.
- Candidate-pool recall remained **0.6078** for every arm because candidate membership never changed.

Pure JEV improved top-five performance but was generally weaker than the hybrid. This suggests that JEV worked best as a component inside the existing ranking policy, not as a total replacement for it.

## 5. Answer-quality findings

We generated paired answers from the baseline and hybrid top-five contexts and evaluated them with reference-free RAGAS metrics.

| Metric | Baseline | Hybrid | Mean change | 95% paired bootstrap interval |
|---|---:|---:|---:|---:|
| Faithfulness | 0.9213 | 0.9539 | +0.0326 | [-0.0375, 0.1182] |
| Answer Relevancy | 0.7928 | 0.8422 | +0.0494 | [-0.0207, 0.1566] |

Both averages moved positively, but both intervals crossed zero and paired tests were not significant. The experiment therefore provides no reliable evidence that hybrid JEV improves answer quality.

Answer Relevancy was especially sensitive to one topic. Excluding `halluc-01` reduced the mean difference to **+0.00263**, showing that the apparent gain was not broad or stable.

## 6. Efficiency findings

The 17 JEV scoring calls used 124,516 input tokens and 6,528 output tokens. Recorded cost was approximately **$0.00523**. Median topic-call latency was about **312 ms**, and p95 was about **472 ms**.

These figures make JEV operationally attractive for a reranking step in this small offline experiment. They are not production latency or cost measurements because no live application integration or concurrent traffic test was performed.

The RAGAS evaluation cost approximately **$0.4086** and is evaluation overhead, not JEV serving cost.

## 7. What the evidence supports

The evidence supports this statement:

> On a frozen 17-topic literature-retrieval dataset, citation-partition + JEV was the best tested arm for top-five ranking and also produced the strongest nDCG@10 and MRR@10.

The evidence does not support these stronger claims:

- JEV is always better than the baseline.
- JEV retrieves more relevant papers.
- JEV reliably improves generated-answer quality.
- JEV can replace the complete retrieval and ranking system.
- The result will generalize to production traffic.

## 8. Decision

JEV is a promising reranking component, particularly when combined with citation protection. It is not approved as a production replacement based on this experiment.

The next study should use a larger independently labelled dataset, preregister primary metrics, repeat answer-quality judgments, and require Recall@10 non-regression before a production trial.
