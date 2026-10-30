# 7. Graph-Based versus Traditional RAG: An Empirical Study

**Core question:** On which question types does graph or hybrid retrieval actually beat vector retrieval, by how much, and at what cost?

**Capabilities:** `core` · `citations` · `text` · `derived` (to test content-heavy graph reasoning)

## Goal

This direction is the research question of the whole project, studied directly. Build several retrieval systems over the same corpus and compare them rigorously across a typed question set: vector RAG, graph-only, hybrid, and at least one published GraphRAG method. The output is a careful empirical study, not a single system.

## Example comparisons

- Vector RAG vs. HippoRAG-style PPR vs. hybrid on multi-hop citation questions
- Microsoft GraphRAG global search vs. vector RAG vs. RAPTOR on "what are the main themes in X" questions
- LightRAG vs. vector RAG on content-only questions: does the graph help, hurt or make no difference?
- Effect of graph quality: metadata-only graph vs. graph with LLM-extracted method/task entities

## Systems to compare

At minimum, use B0–B4 from the [evaluation guide](../docs/evaluation.md#1-baselines-every-team-must-report), plus one or more of Microsoft GraphRAG, LightRAG and HippoRAG 2 run on your corpus.

## Suggested approach

| Level | Approach |
| --- | --- |
| Baseline | B0–B4 on a typed question set of at least 150 questions, with per-type results |
| Stronger | Add published GraphRAG systems; ablations (hop depth, k, context budget, embedding model, with/without `derived` entities); efficiency measurements (indexing cost, latency, tokens) |
| Stretch | A **router** that predicts from the question which retriever to use, evaluated against always-vector and always-graph; analysis of *why* each system fails (retrieval vs. reasoning) |

## Evaluation

Everything in the [evaluation guide](../docs/evaluation.md), with particular care for:

- **Per-question-type tables.** This is the main result.
- **Significance**: paired bootstrap or McNemar tests for every claimed improvement
- **Equal context budgets** across systems
- **Cost**: indexing tokens and dollars/time, query latency p50/p95
- **Error analysis**: is a failure caused by retrieval (evidence not in context) or generation (evidence present, answer wrong)?

## Pitfalls

- **Biased question sets.** Questions generated from the graph favour the graph. Include external and hand-written questions.
- **Unequal tuning.** If you tune your graph system for two weeks and the vector baseline for an hour, the comparison means nothing. Give each system the same tuning budget on the dev split.
- **Framework defaults.** Published GraphRAG systems ship with different default prompts, chunk sizes and models. Normalise what you can, and document what you could not.
- **Overclaiming.** "Graph helps on T3 and T7 but not on T6, at 40× the indexing cost" is a good result. Report negative findings.

## Key references

HippoRAG 2 (structure-augmented RAG falling below vector RAG on factual questions), Microsoft GraphRAG, LightRAG, RAPTOR, both GraphRAG surveys, STaRK, Ragas (see [literature](../literature/README.md)).
