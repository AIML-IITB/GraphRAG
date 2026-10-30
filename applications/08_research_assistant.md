# 8. Intelligent Research Assistant

**Core question:** Can an LLM agent decide for itself when to use graph queries, semantic search, citation traversal or analytics, and complete realistic literature-review tasks reliably and efficiently?

**Capabilities:** `core` · `citations` · `text` · `derived` (optional)

## Goal

Build an LLM-powered assistant that supports multi-step research tasks by calling tools over the scholarly knowledge graph and the text index. Each answer should show the evidence it used and the steps it took.

## Example tasks

- "Give me a structured literature review of graph-based RAG: key papers, main approaches, open problems."
- "I have this abstract [pasted]. Find the 10 most relevant prior works and tell me how my approach differs."
- "Who are the main groups working on scientific QA, and how are they connected?"
- "Find papers that cite both X and Y, and summarise how they combine the two ideas."

## Tools to expose to the agent

| Tool | Backed by |
| --- | --- |
| `search_papers(query, filters)` | Vector / BM25 index over paper text |
| `get_paper(id)` | Node lookup (metadata, abstract) |
| `get_citations(id)` / `get_references(id)` | `CITES` edges |
| `graph_query(cypher)` or templated traversals | Graph store |
| `find_path(a, b)` | Citation / co-authorship path search |
| `rank(entity_type, topic)` | Analytics from directions 2 and 4 |
| `summarise(papers)` | LLM over the selected papers |

## Suggested approach

| Level | Approach |
| --- | --- |
| Baseline | Single-shot: one retrieval call (vector RAG), then answer |
| Stronger | A tool-calling agent (ReAct-style loop) with the tools above, plus a planner that decomposes the task. All tool calls are logged and every claim carries a citation. |
| Stretch | Self-verification (check each cited paper supports its claim); memory across a session; a learned or heuristic tool router; a user interface |

## Evaluation

- **Task success rate** on a set of 30+ realistic tasks with rubrics (written before running the system)
- **Evidence grounding**: fraction of claims supported by the cited papers
- **Tool-use efficiency**: tool calls per task, redundant or failed calls, tokens and cost per task, wall-clock time
- **Comparison with baselines**: single-shot vector RAG, and the agent with graph tools removed (an ablation that isolates the value of graph tools)

## Pitfalls

- **Agents loop and wander.** Cap steps and budget, and log everything.
- **Tool descriptions matter more than you expect.** A vague `graph_query` description leads to bad Cypher. Iterate on the descriptions using the dev tasks.
- **Rubric drift**: write task rubrics before looking at outputs.
- **Evaluation cost.** Agent runs are expensive and noisy, so run each task several times and report variance.

## Key references

Think-on-Graph, both GraphRAG surveys, neo4j-graphrag (text-to-Cypher), Ragas (see [literature](../literature/README.md) and [resources](../resources/README.md)).
