# Application Directions

Every team builds the [minimum working system](../docs/problem_statement.md#minimum-working-system-required-for-all-teams). Then pick **one or two** of these directions and go deep. Combining directions is encouraged (e.g. 1 + 7, 3 + 6, 8 + anything). So are your own directions, provided you can justify them.

| # | Direction | Core question | Capabilities needed | Primary evaluation |
| --- | --- | --- | --- | --- |
| 1 | [Multi-hop scholarly QA](01_multi_hop_qa.md) | Answer questions whose evidence spans several connected entities | `core` `citations` `text` | Answer EM/F1, evidence recall, hop-wise accuracy |
| 2 | [Venue & researcher ranking](02_venue_researcher_ranking.md) | Rank venues and researchers (overall and per topic) with explanations | `core` `citations` (+`affiliations`) | Rank correlation vs. CORE and bibliometric indices |
| 3 | [Paper recommendation](03_paper_recommendation.md) | Recommend papers using text + citation structure | `core` `citations` `text` | Precision/Recall/nDCG@K on held-out citations, LitSearch |
| 4 | [Trend analysis](04_trend_analysis.md) | Detect and explain how topics emerge, merge and decline | `core` `text` `derived` | Topic coherence, temporal validity, expert assessment |
| 5 | [Collaboration discovery](05_collaboration_discovery.md) | Find experts, communities and future collaborators | `core` `text` | Temporal link prediction (AUC, Hits@K), expert finding |
| 6 | [Citation path exploration](06_citation_path_exploration.md) | Find and explain how papers or areas are connected | `core` `citations` `text` | Path relevance, factual validity of explanations |
| 7 | [Graph vs. vector RAG](07_graph_vs_vector_rag.md) | Which question types benefit from graph structure, and at what cost? | `core` `citations` `text` `derived` | Per-question-type answer and retrieval metrics, latency, cost |
| 8 | [Research assistant](08_research_assistant.md) | An agent that picks graph or text tools to complete research tasks | `core` `citations` `text` | Task success, grounding, tool efficiency |

Capabilities are defined in [dataset/README.md](../dataset/README.md). `core` alone is **not enough for any direction**. Every direction needs either `citations` or `text` on top of it.

## Each brief contains

- **Goal** and **example queries**
- **Graph signals** to exploit
- **Suggested approach** at three levels (baseline → stronger → stretch)
- **Evaluation** and **pitfalls**
- **Key references**

The suggested approaches are starting points, not requirements. A well-evaluated simple system beats a poorly evaluated complex one.
