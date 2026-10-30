# 3. Research Paper Discovery and Recommendation

**Core question:** Does adding citation and co-authorship structure to semantic similarity produce better paper recommendations than text similarity alone?

**Capabilities:** `core` · `citations` · `text`

## Goal

Given a query (free text, a seed paper, a set of papers, or a researcher profile), recommend relevant papers and explain each recommendation, e.g. "co-cited with your seed paper 14 times; uses the same method".

## Example queries

- "I am writing a paper on graph-based retrieval for scientific QA. What should I cite?"
- "Papers similar to *From Local to Global* that I might have missed"
- "Recommend recent (2024+) papers for someone whose work is on citation recommendation."
- "What are the foundational papers I should read before reading HippoRAG?" (backward citation traversal)

## Graph signals

- **Co-citation** (cited together) and **bibliographic coupling** (cite the same papers)
- Personalized PageRank from seed papers
- Author / venue proximity
- Node embeddings (node2vec, GNN) combined with text embeddings (SPECTER2)

## Suggested approach

| Level | Approach |
| --- | --- |
| Baseline | Text-only: BM25 and SPECTER2 nearest neighbours on title + abstract |
| Stronger | Re-rank text candidates using graph features (co-citation, coupling, PPR score); or hybrid rank fusion of text and graph retrievers. Generate an LLM explanation from the shared graph evidence. |
| Stretch | Learned ranker over text + graph features; GNN link prediction (paper–paper citation); cold-start handling for new papers with no citations yet |

## Evaluation

- **Held-out citation prediction**: hide a paper's references and check whether they are recommended. Precision@K, Recall@K, nDCG@K, MRR.
- **LitSearch** queries with gold papers
- **Beyond accuracy**: coverage, novelty (non-obvious recommendations), recency
- **Explanations**: human rating of whether they are faithful to the graph evidence

## Pitfalls

- **Temporal leakage.** When predicting citations of a paper from year *t*, use only papers and edges available before *t*.
- **Popularity bias.** Graph signals favour highly cited papers. Report results for long-tail target papers separately.
- Held-out citations are an imperfect ground truth, because authors miss relevant work. Complement them with a small human judgement set.

## Key references

SPECTER / SciRepEval, LitSearch, node2vec, GraphSAGE, HippoRAG (PPR) (see [literature](../literature/README.md)).
