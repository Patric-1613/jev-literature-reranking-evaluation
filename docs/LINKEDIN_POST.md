# LinkedIn Post Draft

I recently tested whether TypeSafe AI's JEV could improve paper ranking inside a literature-review RAG pipeline.

Instead of changing the production architecture immediately, I built a frozen evaluation:

- 17 human-reviewed research questions
- 20 candidates per question
- 340 candidate rows
- identical candidate membership across every ranking arm
- comparisons between the current ranking, pure JEV, and citation-partition + JEV

The hybrid approach produced the strongest early-ranking results:

- Recall@5: **0.1176 → 0.5000**
- Precision@5: **0.0235 → 0.1647**
- MRR@10: **0.1662 → 0.4085**
- nDCG@10: **0.2712 → 0.4111**

But the experiment also showed why evaluation needs more than one headline metric:

- Recall@10 declined slightly: **0.6078 → 0.5784**
- candidate-pool recall could not improve because reranking does not retrieve missing papers
- RAGAS Faithfulness and Answer Relevancy moved positively, but the differences were not statistically reliable
- most of the Answer Relevancy gain came from one topic

My conclusion is not that JEV should replace the current retrieval system. The evidence suggests something more specific: **JEV may be useful as a low-cost reranking component for improving the first few results, especially when combined with existing citation rules.**

The next step would be a larger, preregistered comparison with at least 100 independently labelled topics, an established reranker baseline, and a Recall@10 non-regression requirement.

I have documented the methodology, frozen evidence hashes, results, limitations, and reproducibility approach here:

https://github.com/Patric-1613/jev-literature-reranking-evaluation

#RAG #InformationRetrieval #AIEngineering #LLMEvaluation #JEV #Reranking #ResearchEngineering

## Short alternative

Can a decision model improve the papers a RAG system shows first?

I tested JEV as a reranker over 17 reviewed literature-search topics and 340 frozen candidates. A citation-aware hybrid increased Recall@5 from **0.1176 to 0.5000** and MRR@10 from **0.1662 to 0.4085**.

The caveat matters: Recall@10 declined slightly, candidate recall did not change, and paired RAGAS gains were inconclusive. So the result is promising for early ranking, not evidence that JEV should replace retrieval.

Methodology, evidence hashes, and limitations: https://github.com/Patric-1613/jev-literature-reranking-evaluation

#RAG #InformationRetrieval #JEV #LLMEvaluation
