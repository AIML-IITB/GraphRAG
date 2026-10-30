# Resources

Tutorials, documentation and libraries. For papers see [literature](../literature/README.md). For data see [dataset](../dataset/README.md).

## Conceptual introductions

- [Microsoft Research: GraphRAG — Unlocking LLM discovery on narrative private data](https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/). An accessible explanation of why graph-structured retrieval helps on "global" questions.
- [Weaviate: Exploring RAG and GraphRAG](https://weaviate.io/blog/graph-rag). When to use vector RAG, GraphRAG, or both.
- [Neo4j: Build a RAG system on a knowledge graph](https://neo4j.com/blog/developer/rag-tutorial/). A hands-on tutorial combining graph queries, embeddings and LLM generation.

## GraphRAG frameworks

Use these for baselines and ideas, or as components. Your report must explain the retrieval you actually use.

| Framework | What it gives you |
| --- | --- |
| [Microsoft GraphRAG](https://github.com/microsoft/graphrag) · [docs](https://microsoft.github.io/graphrag/) | LLM entity/relation extraction, Leiden communities, community reports, local / global / DRIFT search. Indexing is LLM-heavy, so estimate cost on a small sample first. |
| [LightRAG](https://github.com/HKUDS/LightRAG) | Lightweight graph + vector retrieval with dual-level keys and incremental updates |
| [HippoRAG](https://github.com/OSU-NLP-Group/HippoRAG) | Open-IE knowledge graph + Personalized PageRank retrieval (HippoRAG 1 and 2) |
| [neo4j-graphrag-python](https://github.com/neo4j/neo4j-graphrag-python) · [docs](https://neo4j.com/docs/neo4j-graphrag-python/current/) | Official Neo4j package: KG construction, vector + Cypher retrievers, text-to-Cypher |
| [LlamaIndex Property Graph Index](https://docs.llamaindex.ai/en/stable/module_guides/indexing/lpg_index_guide/) | Property-graph construction and multiple graph retrievers within LlamaIndex |

## Graph libraries

| Library | Use |
| --- | --- |
| [NetworkX](https://networkx.org/) | Simplest API: PageRank, shortest paths, communities. Slow beyond ~10⁶ edges. |
| [igraph (Python)](https://python.igraph.org/) | Much faster C core; Leiden community detection |
| [PyTorch Geometric](https://pytorch-geometric.readthedocs.io/) | Heterogeneous GNNs, link prediction, node2vec |
| [Neo4j](https://neo4j.com/) | Graph database with Cypher, the Graph Data Science library and a vector index |

## Retrieval and embeddings

- [SPECTER2](https://huggingface.co/allenai/specter2): scientific paper embeddings (title + abstract)
- [sentence-transformers](https://www.sbert.net/): general-purpose embedding models
- [FAISS](https://github.com/facebookresearch/faiss): vector similarity search
- [Pyserini](https://github.com/castorini/pyserini): BM25 and other sparse retrieval baselines

## LLM serving

- [Ollama](https://ollama.com/): run open models locally with one command
- [vLLM](https://github.com/vllm-project/vllm): high-throughput serving for open models, useful for bulk entity extraction

## Evaluation

- [Ragas](https://github.com/explodinggradients/ragas): RAG metrics (faithfulness, context precision/recall)
- [STaRK](https://stark.stanford.edu/): semi-structured retrieval benchmark, including a scholarly graph split
- [LitSearch](https://github.com/princeton-nlp/LitSearch): literature search benchmark

## Data source documentation

- [dblp XML downloads](https://dblp.org/xml/) and [monthly snapshot releases](https://dblp.org/xml/release/)
- [Semantic Scholar API overview](https://www.semanticscholar.org/product/api) · [Graph API reference](https://api.semanticscholar.org/api-docs/graph) · [Datasets API reference](https://api.semanticscholar.org/api-docs/datasets) · [API tutorial](https://www.semanticscholar.org/product/api/tutorial)
- [OpenAlex documentation](https://docs.openalex.org/)
- [CORE conference rankings portal](https://portal.core.edu.au/conf-ranks/)
