# 6. Citation Path Exploration and Explainable Research Discovery

**Core question:** Can the system find and *explain* meaningful connections between papers or research areas, and are those explanations factually correct?

**Capabilities:** `core` · `citations` · `text`

## Goal

Given two papers, two topics or two researchers, find the citation chains and intermediate works that connect them, and explain in natural language how the ideas flowed. Also identify the influential papers that link otherwise separate areas.

## Example queries

- "How is word2vec connected to retrieval-augmented generation?"
- "Trace the lineage of ideas from PageRank to HippoRAG."
- "Which papers bridge knowledge graph embedding research and LLM-based question answering?"
- "Why does paper X cite paper Y? What did it take from it?"

## Graph signals

- Shortest and k-shortest citation paths, and **diverse** paths
- Main-path analysis (search path count) on the citation DAG
- Betweenness centrality for bridge papers
- Citation context: the sentence in the citing paper that mentions the cited paper (needs full text). Without full text, use the two abstracts.

## Suggested approach

| Level | Approach |
| --- | --- |
| Baseline | Shortest citation path between two papers; the LLM explains each hop using the two papers' abstracts |
| Stronger | Rank candidate paths by relevance (semantic coherence between consecutive papers, citation importance, recency) instead of length alone. Prune paths through generic hub papers (surveys, Adam, ResNet, …). Explain the whole path as a narrative, with evidence at each hop. |
| Stretch | Prize-collecting Steiner tree subgraphs connecting several seeds (G-Retriever style); citation-intent classification for each edge (uses / extends / compares); an interactive exploration UI |

## Evaluation

- **Path relevance**: human rating of whether each path is a meaningful line of influence
- **Factual validity**: for each hop's explanation, does the claimed relationship actually hold? Check against the abstract or citation context.
- **Explainability**: user-study style ratings (clarity, usefulness) on a small set of queries
- Compare against a vector-RAG LLM asked the same "how is A connected to B" question. Does it invent connections?

## Pitfalls

- **Hub papers** make every two papers 2–3 hops apart, so the shortest path is usually uninformative. Path ranking matters more than path finding.
- **Hallucinated explanations**: the LLM will happily explain a connection that the papers do not support. Ground each hop on actual text.
- Citations do not imply influence. Many citations are perfunctory.

## Key references

G-Retriever, Think-on-Graph, GRAG, SPECTER (see [literature](../literature/README.md)).
