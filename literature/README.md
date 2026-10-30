# Literature

Read in tiers. Everyone should read Tier 1 in week 1. After that, read Tiers 2 and 3 based on the direction you choose.

## Tier 1: Core reading (everyone)

These six papers go from understanding GraphRAG to implementing graph-based retrieval over scholarly data.

| # | Paper | Why read it |
| --- | --- | --- |
| 1 | **Retrieval-Augmented Generation with Graphs (GraphRAG)**. Han et al., 2024. [arXiv:2501.00309](https://arxiv.org/abs/2501.00309) | A broad survey of GraphRAG: graph types, retrievers, how retrieved subgraphs are fed to LLMs, and open challenges. **Start here** to get the vocabulary and the design space. |
| 2 | **From Local to Global: A Graph RAG Approach to Query-Focused Summarization**. Edge et al., 2024. [arXiv:2404.16130](https://arxiv.org/abs/2404.16130) | Microsoft's GraphRAG. An LLM extracts an entity graph, the graph is partitioned into communities (Leiden), and the communities are summarised. **Global** questions over a whole corpus are answered from the community summaries. Relevant to trend analysis and to questions like "what are the main themes in X". Note the indexing cost. |
| 3 | **HippoRAG: Neurobiologically Inspired Long-Term Memory for Large Language Models**. Gutiérrez et al., NeurIPS 2024. [arXiv:2405.14831](https://arxiv.org/abs/2405.14831) | Builds an open knowledge graph from passages and retrieves with **Personalized PageRank** seeded by query entities. A simple and effective recipe for **multi-hop** retrieval, and a strong baseline to reimplement. |
| 4 | **GRAG: Graph Retrieval-Augmented Generation**. Hu et al., 2024. [arXiv:2405.16506](https://arxiv.org/abs/2405.16506) | Retrieves **textual subgraphs** rather than isolated documents, and combines text with graph topology when generating. It is motivated by networked documents such as citation graphs, which is close to this project, although its experiments use general graph-reasoning benchmarks. |
| 5 | **LightRAG: Simple and Fast Retrieval-Augmented Generation**. Guo et al., Findings of EMNLP 2025. [ACL Anthology](https://aclanthology.org/2025.findings-emnlp.568/) · [arXiv:2410.05779](https://arxiv.org/abs/2410.05779) | A cheaper alternative to Microsoft GraphRAG. **Dual-level retrieval** combines low-level (specific entities) and high-level (themes) keys, and supports incremental index updates. A good template for an efficient hybrid system. |
| 6 | **The Semantic Scholar Open Data Platform**. Kinney et al., 2023. [arXiv:2301.10140](https://arxiv.org/abs/2301.10140) | How the Semantic Scholar Academic Graph is built: paper and author disambiguation, citation linking, and the APIs and datasets. Read it to understand **where your citation edges come from and how noisy they are**. |

## Tier 2: Methods (by direction)

| Paper | Relevant to |
| --- | --- |
| **Graph Retrieval-Augmented Generation: A Survey**. Peng et al., 2024. [arXiv:2408.08921](https://arxiv.org/abs/2408.08921) | A second survey, organised as G-Indexing → G-Retrieval → G-Generation. Useful for structuring your design section. |
| **From RAG to Memory: Non-Parametric Continual Learning for LLMs** (HippoRAG 2). Gutiérrez et al., ICML 2025. [arXiv:2502.14802](https://arxiv.org/abs/2502.14802) | Multi-hop QA (1), graph vs. vector (7). Its key observation: structure-augmented RAG methods often **fall below plain vector RAG on simple factual questions**. HippoRAG 2 fixes this with deeper passage integration on top of PPR. Read it before you design your comparison. |
| **G-Retriever: Retrieval-Augmented Generation for Textual Graph Understanding and Question Answering**. He et al., 2024. [arXiv:2402.07630](https://arxiv.org/abs/2402.07630) | Subgraph retrieval formulated as a Prize-Collecting Steiner Tree, with a GNN encoder plus LLM. Multi-hop QA (1) and citation paths (6). |
| **Think-on-Graph: Deep and Responsible Reasoning of LLM on Knowledge Graph**. Sun et al., ICLR 2024. [arXiv:2307.07697](https://arxiv.org/abs/2307.07697) | An LLM agent that iteratively explores the KG with beam search over paths. Research assistant (8) and citation paths (6). |
| **RAPTOR: Recursive Abstractive Processing for Tree-Organized Retrieval**. Sarthi et al., 2024. [arXiv:2401.18059](https://arxiv.org/abs/2401.18059) | A hierarchical summary tree. A non-graph alternative to community summaries for global and trend questions (4). |
| **From Louvain to Leiden: Guaranteeing Well-Connected Communities**. Traag et al., 2018. [arXiv:1810.08473](https://arxiv.org/abs/1810.08473) | The community detection algorithm used by Microsoft GraphRAG. Trends (4) and collaboration communities (5). |
| **node2vec: Scalable Feature Learning for Networks**. Grover & Leskovec, 2016. [arXiv:1607.00653](https://arxiv.org/abs/1607.00653) | Node embeddings for recommendation (3) and link prediction (5) |
| **Inductive Representation Learning on Large Graphs** (GraphSAGE). Hamilton et al., 2017. [arXiv:1706.02216](https://arxiv.org/abs/1706.02216) | GNN embeddings for new nodes (papers arriving over time). Recommendation (3) and collaboration (5). |
| **An index to quantify an individual's scientific research output** (h-index). Hirsch, 2005. [arXiv:physics/0508025](https://arxiv.org/abs/physics/0508025) | Bibliometric baseline for researcher ranking (2) |

## Tier 3: Scholarly data, representations and evaluation

| Paper | Why |
| --- | --- |
| **SPECTER: Document-level Representation Learning using Citation-informed Transformers**. Cohan et al., ACL 2020. [arXiv:2004.07180](https://arxiv.org/abs/2004.07180) | Paper embeddings trained on citations, which makes them a strong vector-RAG baseline for scholarly text. Use the newer [SPECTER2](https://huggingface.co/allenai/specter2) checkpoint. |
| **SciRepEval: A Multi-Format Benchmark for Scientific Document Representations**. Singh et al., EMNLP 2023. [arXiv:2211.13308](https://arxiv.org/abs/2211.13308) | Introduces SPECTER2 and benchmarks scientific embeddings. Helps you choose an embedding model. |
| **STaRK: Benchmarking LLM Retrieval on Textual and Relational Knowledge Bases**. Wu et al., NeurIPS 2024 Datasets & Benchmarks. [arXiv:2404.13207](https://arxiv.org/abs/2404.13207) | **Ground-truth evaluation** for queries mixing text and relations. Its MAG split is a scholarly graph. |
| **LitSearch: A Retrieval Benchmark for Scientific Literature Search**. Ajith et al., EMNLP 2024. [arXiv:2407.18940](https://arxiv.org/abs/2407.18940) | Ground truth for literature search and recommendation |
| **MuSiQue: Multihop Questions via Single-hop Question Composition**. Trivedi et al., TACL 2022. [arXiv:2108.00573](https://arxiv.org/abs/2108.00573) | How to build multi-hop questions that cannot be shortcut. Useful when designing your own evaluation set. |
| **Ragas: Automated Evaluation of Retrieval Augmented Generation**. Es et al., 2023. [arXiv:2309.15217](https://arxiv.org/abs/2309.15217) | Reference-free RAG metrics (faithfulness, answer relevance, context relevance). Validate against human labels before relying on them. |
| **OpenAlex: A fully-open index of scholarly works, authors, venues, institutions, and concepts**. Priem et al., 2022. [arXiv:2205.01833](https://arxiv.org/abs/2205.01833) | An alternative open scholarly graph with institutions and concepts |
| **Open Graph Benchmark: Datasets for Machine Learning on Graphs**. Hu et al., NeurIPS 2020. [arXiv:2005.00687](https://arxiv.org/abs/2005.00687) | `ogbn-mag` / `ogbn-arxiv`: ready-made scholarly graphs with fixed splits for graph-ML components |
| **Scholarly knowledge graphs through structuring scholarly communication: a review**. Complex & Intelligent Systems, 2022. [Springer](https://link.springer.com/article/10.1007/s40747-022-00806-6) | Survey of scholarly KG schemas and projects. Useful when designing your schema. |

## How to read a paper for this project

For each paper, write down four things in your team notes:

1. **What graph** does it build (nodes, edges), and is it extracted by an LLM or taken from structured data?
2. **How does it retrieve**: traversal, PPR, community summaries, subgraph search, or an agent?
3. **What does it compare against**, and on which question types does it win or lose?
4. **What does it cost** to index and to query?

These four questions map directly onto the design and evaluation sections of your report.
