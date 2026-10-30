# Evaluation

A GraphRAG system is only as convincing as its comparison. This page defines the baselines every team reports, how to build a question set, the metrics for each direction, and the rules that keep comparisons fair.

## 1. Baselines every team must report

Whatever direction you choose, compare your system against these, using the **same LLM, the same corpus slice and the same context budget**:

| # | Baseline | What it is | Why it is there |
| --- | --- | --- | --- |
| B0 | **Closed-book LLM** | The question goes to the LLM with no retrieval | LLMs have memorised many famous papers. If B0 scores well, your questions test memory, not retrieval. |
| B1 | **Sparse retrieval** | BM25 over paper text (title + abstract) → LLM | A strong, cheap lexical baseline |
| B2 | **Vector RAG** | Dense embeddings (e.g. SPECTER2 or a general sentence encoder) over paper text → top-k → LLM | The "ordinary RAG" this project is asking you to beat |
| B3 | **Graph retrieval** | Your graph-based retrieval only (traversal, Cypher, PPR, community summaries, …) → LLM | Isolates the contribution of structure |
| B4 | **Hybrid** | Vector + graph (e.g. vector seeds expanded by graph neighbourhood, or rank fusion) | Usually the strongest practical system |

> B1/B2 need the `text` capability. Embedding titles alone does not count as a vector-RAG baseline.

## 2. Question taxonomy

Tag **every** evaluation question with one type, and report all results **per type**, not just overall. The per-type breakdown is the research result.

| Type | Example | Needs |
| --- | --- | --- |
| **T1 Lookup** (1-hop, structured) | "Which venue published *Attention Is All You Need*?" | `core` |
| **T2 Multi-hop, structured** | "Which authors have co-authored with someone who has published at both ACL and KDD?" | `core` |
| **T3 Citation-structured** | "Which papers cite both GraphSAGE and node2vec?" | `citations` |
| **T4 Aggregation / global** | "Which institutions published most on retrieval-augmented generation in 2023–2024?" | `core` + `derived` |
| **T5 Temporal** | "How did the methods used for scientific claim verification change between 2019 and 2024?" | `core` + `text`/`derived` |
| **T6 Content (single-paper)** | "What dataset does the LightRAG paper evaluate on?" | `text` |
| **T7 Content × structure** | "Among papers citing HippoRAG, which ones propose a different graph-traversal strategy than PPR?" | `citations` + `text` |

What to expect: vector RAG should be competitive on T6, graph retrieval should dominate T1–T3, and **T4, T5 and T7 are where the real question lies**. A project that evaluates only T1–T3 has not answered the research question.

## 3. Building an evaluation set

Use at least one of these, and preferably two (one external and one your own).

1. **External benchmarks**
   - **STaRK-MAG**: natural-language queries over a scholarly graph with gold answer nodes. Best fit for T2/T3/T7-style retrieval. You will need to align it with your graph, or run your retriever on STaRK's own graph.
   - **LitSearch**: literature-search queries with gold papers. Best fit for recommendation and T6/T7 retrieval.
2. **Templated questions generated from your graph**
   - Write question templates per type, fill them by sampling from the graph, and compute the gold answer by query.
   - This is cheap and gives exact answers, **but it is biased toward graph retrieval**, because the question was born from the graph. Always pair it with a non-templated set.
3. **Hand-written questions**
   - 50–100 questions written by the team *before* looking at system outputs, each with a gold answer and the supporting papers (evidence).
   - Have a second team member verify each gold answer.

**Minimum:** 150 questions across at least 5 of the 7 types, each with a gold answer and gold evidence papers. Release the set in your repository.

## 4. Metrics

### Retrieval quality (all directions)

- **Recall@k / Precision@k / nDCG@k** on gold evidence papers (or nodes)
- **Evidence recall**: fraction of required supporting papers that appear in the context given to the LLM
- **Hop-wise recall** for multi-hop questions: is the evidence found at hop 1, 2, 3?

### Answer quality

- **Exact match / F1** for short answers (names, venues, years, lists)
- **List-answer F1** for set-valued questions
- **LLM-as-judge** for long answers (correctness, completeness, groundedness). Use a *different* model from the one generating answers, publish the judge prompt, and validate the judge against human labels on at least 50 items (report agreement).
- **Citation / grounding accuracy**: does every cited paper exist, and does it actually support the claim?

### Efficiency

- **End-to-end latency** (p50 / p95), split into retrieval and generation time
- **Indexing cost**: time and LLM tokens to build the index or graph. GraphRAG-style indexing can be expensive, so report it.
- **Tokens per query** sent to the LLM

### Direction-specific metrics

| # | Direction | Primary evaluation |
| --- | --- | --- |
| 1 | Multi-hop scholarly QA | Answer EM/F1, evidence recall, hop-wise performance, per-question-type breakdown |
| 2 | Venue & researcher ranking | Rank correlation (Spearman ρ, Kendall τ) against external rankings (e.g. CORE for conferences) and bibliometric indices (h-index, citation counts); stability across time windows |
| 3 | Paper recommendation | Precision@K, Recall@K, nDCG@K, MRR on held-out citations or LitSearch; coverage and novelty |
| 4 | Trend analysis | Topic coherence (e.g. NPMI), temporal validity (do detected emergence dates match known events?), expert assessment |
| 5 | Collaboration discovery | Link-prediction metrics on a temporal split (AUC, Hits@K, MRR); qualitative relevance of suggestions |
| 6 | Citation path exploration | Path relevance (human-rated), factual validity of each hop's explanation, explainability rating |
| 7 | Graph vs. vector RAG | All of the retrieval, answer and efficiency metrics above, broken down per question type, with significance tests |
| 8 | Research assistant | Task success rate, evidence grounding, tool-call efficiency (calls per task, wasted calls), cost per task |

## 5. Fairness rules

- **Same generator LLM, same temperature, same prompt template** across B0–B4 and your system. Only the retrieved context differs.
- **Same context budget**: fix a token budget (e.g. 4k tokens) for retrieved context. Graph methods must not win simply by stuffing more text into the prompt.
- **No test-set tuning**: split your questions into dev and test. Tune k, hop depth, prompts etc. on dev only.
- **Temporal splits for prediction tasks**: train on edges up to year *t*, test on edges after *t*. Random edge splits leak the future.
- **Report variance**: run stochastic components at least 3 times. Report 95% bootstrap confidence intervals, and use a paired test (e.g. paired bootstrap or McNemar) for system-vs-baseline claims.

## 6. What to put in the report

- One table: systems × question types, for your main answer metric
- One table: systems × retrieval metrics
- One table: latency and cost
- An **error analysis** of at least 30 failures, categorised (retrieval miss, wrong hop, entity-linking error, LLM reasoning error, gold-answer error)
- A clear statement of **which question types benefited from graph structure, and which did not**
