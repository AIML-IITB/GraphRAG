# 1. Multi-Hop Scholarly Question Answering

**Core question:** Can the system answer questions whose answer is spread across several connected papers, authors and venues, and show the evidence chain?

**Capabilities:** `core` · `citations` · `text` · `derived` (optional)

## Goal

Answer natural-language questions that require traversing papers, authors, citations, venues and topics, and return the answer together with the supporting papers and the path that connects them.

## Example queries

- "Which authors of papers citing *HippoRAG* have also published at SIGIR?"
- "What methods were proposed by researchers who previously co-authored with the authors of SPECTER?"
- "Which venues published the most papers that cite both GraphSAGE and node2vec?"
- "Among papers citing LightRAG, which evaluate on a scientific-literature dataset?" (needs `text`)
- "Did any first author of an ACL 2023 best paper later publish on graph-based retrieval?"

## Graph signals

- Typed multi-hop paths (Author → Paper → CITES → Paper → Venue)
- Entity neighbourhoods around linked query entities
- Personalized PageRank from seed entities (HippoRAG-style)
- Text attached to paper nodes, for the content constraints in a question

## Suggested approach

| Level | Approach |
| --- | --- |
| Baseline | Entity linking (string / embedding match to paper titles and author names) → fixed k-hop neighbourhood → serialise as text → LLM |
| Stronger | **Query decomposition**: the LLM splits the question into sub-questions, each answered by a graph query (text-to-Cypher or templated traversal) or vector search, then composes the result. Or **PPR retrieval** seeded from linked entities. |
| Stretch | Subgraph retrieval with learned scoring (G-Retriever / GRAG style); iterative LLM-driven exploration (Think-on-Graph); verifying each hop against paper text |

## Evaluation

- Answer EM / F1 (short and list answers), evidence recall, **hop-wise** accuracy (1, 2, 3+ hops)
- Per-question-type results (T1–T7 in the [evaluation guide](../docs/evaluation.md#2-question-taxonomy))
- External benchmark: STaRK-MAG; your own templated + hand-written sets
- Error categories: entity-linking failure, wrong hop, missing edge, LLM reasoning error

## Pitfalls

- **Entity linking is the bottleneck.** "the SPECTER paper" and "Cohan et al. 2020" must resolve to the same node. Measure linking accuracy separately.
- Text-to-Cypher fails silently by returning an empty result or the wrong one. Log every generated query and inspect failures.
- Neighbourhoods explode for high-degree nodes (e.g. a paper cited 50,000 times). Cap or rank expansions.
- The closed-book LLM (B0) may already know answers about famous papers, so include questions about less well-known work.

## Key references

HippoRAG and HippoRAG 2, GRAG, G-Retriever, Think-on-Graph, STaRK, MuSiQue (see [literature](../literature/README.md)).
