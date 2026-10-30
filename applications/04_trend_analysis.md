# 4. Research Trend Analysis and Knowledge Evolution

**Core question:** Can the graph reveal how research topics emerge, grow, merge, split and decline, and can an LLM explain those changes using real papers as evidence?

**Capabilities:** `core` · `text` · `derived` · `citations` (for influence flows)

## Goal

Detect and explain the evolution of research topics over time. Answer "global" questions about the corpus that no single document answers.

## Example queries

- "How did retrieval-augmented generation evolve from 2020 to 2025?"
- "Which topics emerged in information retrieval after 2022, and which papers started them?"
- "Did graph neural networks and knowledge graph embedding merge into one research area?"
- "Which research directions in NLP are declining?"
- "What are the main themes of papers that cite *Attention Is All You Need*, per year?"

## Graph signals

- **Topic communities**: Leiden / Louvain communities on citation or co-citation graphs, per time window
- **Community tracking** across windows: birth, growth, merge, split, death
- **Derived topic or method nodes** and their paper counts over time
- **Citation flows between topics**: which topic builds on which
- **Community summaries** (Microsoft GraphRAG style) for global questions

## Suggested approach

| Level | Approach |
| --- | --- |
| Baseline | Topic modelling (e.g. BERTopic or LDA) on abstracts with yearly counts. A vector-RAG LLM answers trend questions from top-k abstracts. |
| Stronger | Time-sliced community detection on the citation graph, with LLM-generated community summaries per slice; match communities across slices to detect events. Answer global questions map-reduce style over the summaries. |
| Stretch | Method/task extraction to track *what techniques* move between areas; predicting emerging topics; comparing graph communities against text topics |

## Evaluation

- **Topic quality**: coherence (NPMI), diversity
- **Temporal validity**: do detected emergence and merge dates match known events? Build a small list of well-documented ones (e.g. the rise of transformers, RAG, diffusion models).
- **Global-question answers**: LLM-judge pairwise comparisons on comprehensiveness, diversity and groundedness (as in the Microsoft GraphRAG paper), validated with human ratings
- **Expert assessment** of a sample of detected trends

## Pitfalls

- Community detection is unstable. Run several seeds and report stability before you claim a "merge".
- Paper counts grow every year, so normalise counts by yearly corpus size.
- Recent years are under-cited (citations take time). Do not read low recent citation counts as decline.
- LLM summaries can invent trends. Every claim in a summary should cite papers from the community.

## Key references

Microsoft GraphRAG (Edge et al.), Leiden algorithm, RAPTOR, LightRAG (high-level retrieval) (see [literature](../literature/README.md)).
