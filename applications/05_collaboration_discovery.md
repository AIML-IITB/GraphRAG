# 5. Researcher and Collaboration Discovery

**Core question:** Can the co-authorship and citation graphs, combined with the content of each researcher's papers, identify relevant experts, communities and promising future collaborations?

**Capabilities:** `core` · `text` · `citations` (optional) · `affiliations` (optional)

## Goal

Find experts on a topic, map research communities, and recommend potential collaborators. Explain each suggestion through the graph (shared co-authors, citing each other, complementary methods).

## Example queries

- "Who are the experts on scientific claim verification?"
- "Suggest potential collaborators for a researcher working on graph-based retrieval who wants to apply it to biomedical literature."
- "Which research communities connect NLP and databases, and who are the bridge researchers?"
- "Find researchers whose work is similar to mine but who are more than three co-authorship hops away."

## Graph signals

- **Co-authorship graph** (`COLLABORATED_WITH`, weighted by number of joint papers and recency)
- Link-prediction heuristics: common neighbours, Adamic–Adar, Jaccard, Katz
- Author-to-author citation flows
- Author topic profiles (aggregated text embeddings or derived topics)
- Communities and **bridge nodes** (high betweenness between communities)

## Suggested approach

| Level | Approach |
| --- | --- |
| Baseline | Expert finding: rank authors by text similarity of their papers to the query. Collaboration: common-neighbour / Adamic–Adar scores. |
| Stronger | Combine topic similarity with structural proximity and complementarity (similar topic, different methods, not yet connected). The LLM explains each suggestion using shared papers and paths. |
| Stretch | GNN link prediction on a temporal split; interdisciplinary bridge detection; institution-level collaboration analysis |

## Evaluation

- **Link prediction on a temporal split**: train on co-authorships up to year *t*, predict new co-authorships after *t*. AUC, Hits@K, MRR.
- **Expert finding**: use held-out authorship (given a paper's abstract, are its authors ranked highly?), or a small hand-labelled set of known experts
- **Qualitative**: human rating of whether suggestions are sensible and novel

## Pitfalls

- **Author identity.** Name-based merging creates false experts. Rely on the dataset's disambiguated author IDs and never merge by name alone.
- **Hyper-authored papers** create huge cliques. Cap or down-weight papers with many authors.
- **Temporal leakage** in link prediction, as with any prediction task.
- **Privacy and ethics**: recommending people is sensitive. Use only public bibliographic data, and avoid profiling beyond publications.

## Key references

node2vec, GraphSAGE, Leiden, Open Graph Benchmark (see [literature](../literature/README.md)).
