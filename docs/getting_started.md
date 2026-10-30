# Getting Started

## Prerequisites

You should be comfortable with:

- Python, pandas, and working with large files (streaming, Parquet)
- Basic graph concepts: degree, paths, connected components, PageRank
- Embeddings and nearest-neighbour search
- Calling an LLM (local or via API) and writing prompts

The [literature list](../literature/README.md) covers the rest.

## Recommended stack

These are suggestions, not requirements. Choose what your team knows, and justify the choice in the report.

| Layer | Options |
| --- | --- |
| Parsing | `lxml.etree.iterparse` (streaming XML), `pyarrow` / `polars` for Parquet |
| Graph store / algorithms | NetworkX or igraph (in memory), Neo4j (persistent, Cypher), DuckDB on edge tables |
| Graph ML | PyTorch Geometric (link prediction, GNN embeddings), node2vec |
| Text embeddings | `allenai/specter2` (trained on scientific papers with citation signals), or any `sentence-transformers` model |
| Vector index | FAISS, Chroma, or the Neo4j vector index |
| Sparse retrieval | `rank_bm25`, Pyserini |
| LLM | Any. Local models via Ollama or vLLM keep costs at zero and make latency measurements cleaner. API models are fine too. **Use the same model for all systems you compare.** |
| GraphRAG frameworks | Microsoft GraphRAG, LightRAG, HippoRAG, neo4j-graphrag, LlamaIndex PropertyGraphIndex. Useful for baselines and ideas, but understand what they do, since your report has to explain your retrieval |
| Evaluation | Your own scripts, plus Ragas if useful |

Links for all of these are in [resources](../resources/README.md).

## Suggested plan (8 weeks)

Compress weeks 5–7 for a 6-week run.

| Week | Focus | Checkpoint |
| --- | --- | --- |
| 1 | Read Tier 1 literature. Set up the environment. Download the data (official snapshot or interim DBLP + Semantic Scholar). Choose your corpus slice. | Data loads; basic statistics computed |
| 2 | Build the knowledge graph (`core` + `citations`). Pick your application direction(s). | **Proposal** (1 page): direction, research question, planned evaluation |
| 3 | Baselines B0–B2 working end to end on a few questions | Baselines answer questions with citations |
| 4 | Build the evaluation set (≥150 questions, typed, with gold evidence). First graph retrieval (B3). | **Mid-point review**: graph stats, baseline numbers, eval set |
| 5–6 | Main method for your direction; hybrid (B4); `derived` entities if needed | Your system beats or ties baselines on the dev set, or you know why it does not |
| 7 | Full evaluation on the test set, ablations, error analysis, latency and cost measurements | Results tables complete |
| 8 | Report, demo, presentation | **Final submission** |

## Week-1 checklist

- [ ] Read the [problem statement](problem_statement.md) and the [evaluation guide](evaluation.md), especially the baselines and question taxonomy
- [ ] Skim all eight [application briefs](../applications/README.md) and shortlist two
- [ ] Download the data ([dataset/README.md](../dataset/README.md)); request a Semantic Scholar API key on day one, because it can take time to arrive
- [ ] Stream-parse the data into Parquet / JSONL and compute node and edge counts
- [ ] Read Tier 1 of the [literature](../literature/README.md)
- [ ] Decide on a graph store and an LLM, and fix the LLM for the rest of the project

## Common mistakes

- **Building on titles only.** A vector baseline without abstracts is a strawman. Get the `text` capability.
- **Only asking questions the graph is guaranteed to win.** See the [question taxonomy](evaluation.md#2-question-taxonomy).
- **Ingesting everything.** Start with a slice of 10⁴–10⁵ papers. Scale up only once the pipeline works.
- **Unbounded LLM extraction.** Extracting entities from 50k abstracts with an API model costs real money and time. Estimate tokens first, test on 500 papers, and cache everything.
- **No caching of API calls.** Every Semantic Scholar and LLM call should be cached to disk.
- **Leaking the future.** For any prediction task (recommendation, collaboration, trends), split by time.
- **Committing data.** Keep raw and processed data out of git (see `.gitignore`). Commit the scripts that produce it.
