# 2. Venue and Researcher Ranking

**Core question:** Can graph structure produce rankings of venues and researchers that agree with expert judgement, and can the system explain *why* an entity ranks where it does?

**Capabilities:** `core` · `citations` · `affiliations` (for institution-level rankings) · `text`/`derived` (for topic-specific rankings)

## Goal

Rank publication venues and researchers, overall and **within a topic**, using signals from the citation and co-authorship graphs. Compare the result against established rankings and bibliometric indices, and expose the ranking through natural-language queries with grounded explanations.

## Example queries

- "What are the most influential venues for graph neural network research since 2018?"
- "Who are the leading researchers on retrieval-augmented generation, and what are their key papers?"
- "Rank NLP conferences by the influence of their papers on information retrieval research."
- "Which early-career researchers (first paper after 2018) have the highest citation-weighted influence in knowledge graphs?"
- "Why is venue X ranked above venue Y for topic Z?" (explanation grounded in papers and citations)

## Graph signals

- **Citation counts and bibliometric indices**: h-index, g-index, i10, citations per paper, field-normalised citations
- **Graph centrality**: PageRank / weighted PageRank on the paper citation graph, aggregated to authors or venues; HITS (hubs and authorities); PageRank on a venue-to-venue citation graph
- **Co-authorship centrality**: degree, betweenness, eigenvector centrality
- **Topic conditioning**: restrict to the topic subgraph, or use topic-personalised PageRank
- **Time**: citation velocity and age-normalised metrics, so that recent work is not penalised

## Suggested approach

| Level | Approach |
| --- | --- |
| Baseline | Raw citation counts and h-index per author / venue, computed from the graph |
| Stronger | PageRank / HITS on the citation graph aggregated to venues and authors; topic-personalised PageRank; age normalisation. Answer ranking questions with an LLM that cites the evidence (top papers, citing venues). |
| Stretch | Heterogeneous ranking (joint author–paper–venue random walks); prediction of future impact from early graph signals; fairness analysis (field size, career stage, self-citation) |

## Evaluation

- **Venues:** rank correlation (Spearman ρ, Kendall τ) against [CORE conference rankings](https://portal.core.edu.au/conf-ranks/) (A*/A/B/C). Also measure how well each metric separates the CORE tiers.
- **Researchers:** correlation with external bibliometric indices; agreement with expert lists (e.g. award or fellow lists in a subfield)
- **Robustness:** stability of rankings across time windows and across corpus slices
- **Explanations:** human rating of whether the explanation is correct and grounded in actual papers

## Pitfalls

- **Your slice is not the world.** Citations from outside the corpus slice are missing, so in-slice citation counts differ from global counts. Report both if you can.
- **CORE ranks conferences, not journals.** Handle journals separately, or exclude them.
- **Self-citation and hyper-authorship** inflate author metrics. Report with and without.
- **Ranking people is sensitive.** Present rankings as properties of the data and metric, not as judgements of individuals, and discuss biases (field size, career stage, gaps in data coverage).

## Key references

h-index (Hirsch 2005), Kinney et al. 2023 (how citation data is built, and its quality), OpenAlex (see [literature](../literature/README.md)).
