# ScholarGraphRAG

**Graph-Augmented Retrieval and Knowledge Discovery over Scientific Citation Networks**

*6–8 weeks · Knowledge Graphs · Graph ML · NLP · Information Retrieval · LLMs*

> **Dataset status:** the official computer-science corpus snapshot is **coming soon**. Until then, start with DBLP + Semantic Scholar. See [dataset/](dataset/README.md).

## The problem

Ordinary RAG retrieves text chunks by similarity. Scholarly knowledge, however, is a graph: papers cite papers, authors co-author, venues publish, and topics evolve. **Build a GraphRAG system over a scientific citation network**, and find out where graph structure helps compared with vector retrieval, and where it does not.

Concretely:

- Build a heterogeneous knowledge graph of papers, authors, venues, institutions and topics
- Retrieve graph-aware evidence for natural-language questions and generate grounded, cited answers with an LLM
- Compare graph, vector and hybrid retrieval on answer correctness, retrieval quality, explainability and latency
- Apply the system to one or two open-ended directions

**Research question:** *which question types actually benefit from graph structure?*

Full statement: [docs/problem_statement.md](docs/problem_statement.md)

## Repository map

| Path | What is there |
| --- | --- |
| [docs/problem_statement.md](docs/problem_statement.md) | Background, problem, research question, minimum working system, expected outcomes |
| [docs/getting_started.md](docs/getting_started.md) | Prerequisites, suggested stack, week-by-week plan, common mistakes |
| [docs/knowledge_graph_schema.md](docs/knowledge_graph_schema.md) | Reference node/edge schema; imported vs. derived vs. extracted edges |
| [docs/evaluation.md](docs/evaluation.md) | Required baselines, question taxonomy, metrics, fairness rules |
| [dataset/](dataset/README.md) | Official corpus (coming soon), DBLP XML, Semantic Scholar citations, joining them, other sources |
| [literature/](literature/README.md) | Reading list in three tiers, with what to take from each paper |
| [applications/](applications/README.md) | Eight open-ended application directions, each with a detailed brief |
| [resources/](resources/README.md) | Tutorials, GraphRAG frameworks, graph and retrieval libraries |

## Application directions

| # | Direction |
| --- | --- |
| 1 | [Multi-hop scholarly question answering](applications/01_multi_hop_qa.md) |
| 2 | [Venue and researcher ranking](applications/02_venue_researcher_ranking.md) |
| 3 | [Research paper discovery and recommendation](applications/03_paper_recommendation.md) |
| 4 | [Research trend analysis and knowledge evolution](applications/04_trend_analysis.md) |
| 5 | [Researcher and collaboration discovery](applications/05_collaboration_discovery.md) |
| 6 | [Citation path exploration and explainable discovery](applications/06_citation_path_exploration.md) |
| 7 | [Graph-based versus traditional RAG evaluation](applications/07_graph_vs_vector_rag.md) |
| 8 | [Intelligent research assistant](applications/08_research_assistant.md) |

Pick one or two, combine them, or propose your own.

## Start here

1. Read the [problem statement](docs/problem_statement.md) and the [evaluation guide](docs/evaluation.md).
2. Follow the [week-1 checklist](docs/getting_started.md#week-1-checklist).
3. Read [Tier 1 of the literature](literature/README.md#tier-1-core-reading-everyone).
4. Shortlist two [application directions](applications/README.md).

## Deliverables

| When | Deliverable |
| --- | --- |
| End of week 2 | **Proposal** (1 page): chosen direction(s), research question, corpus slice, evaluation plan |
| Week 4 | **Mid-point review**: graph statistics, baseline results, evaluation set |
| End of project | **Code repository** (reproducible: scripts that fetch and build the data, a README with run instructions, no raw data committed) |
| | **Evaluation set** (questions, gold answers, gold evidence, question types) |
| | **Report** covering design, graph statistics, results per question type, ablations, error analysis, latency/cost, and limitations |
| | **Demo** and **presentation** |

A strong project does not need the most complex system. It needs a working pipeline, a fair comparison, and a clear, evidence-backed answer to *where graph structure helps*.

## Licence

Documentation in this repository is released under the [Apache License 2.0](LICENSE). Datasets referenced here have their own licences. See [dataset/](dataset/README.md).
