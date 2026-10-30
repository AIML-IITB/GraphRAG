# ScholarGraphRAG: Graph-Augmented Retrieval and Knowledge Discovery over Scientific Citation Networks

**Duration:** 6–8 weeks · **Areas:** Knowledge Graphs, Graph Algorithms, Graph ML, NLP, Information Retrieval, Retrieval-Augmented Generation, LLMs

## Background and motivation

Scientific literature is growing faster than any researcher can read. Finding relevant work, understanding how a topic evolved, identifying influential contributions, and seeing how researchers, papers and communities connect are all becoming harder.

Bibliographic databases record far more than text. They record who wrote what, where it was published, and which paper cites which. Together these form a large, **heterogeneous scholarly knowledge graph**: papers, authors, venues, institutions and topics, connected by authorship, citation, co-authorship and publication relationships.

Conventional retrieval relies on keyword matching or semantic similarity. That works well for "find papers about X". It works poorly for questions that need **multi-hop reasoning** ("which groups that cite X also work on Y?"), **aggregation** ("which institutions lead on Z?") or **exploration of connections** ("how is area A related to area B?").

**Graph Retrieval-Augmented Generation (GraphRAG)** combines graph-structured knowledge, information retrieval and large language models (LLMs). Its aim is retrieval that is more context-aware and more explainable.

## Problem statement

Design and develop a **GraphRAG framework for intelligent retrieval, exploration and knowledge discovery over a scientific citation network.**

The framework should:

1. Represent scholarly information as a **heterogeneous knowledge graph**. It should cover at least papers, authors and venues, and ideally institutions, topics and methods, together with their relationships (see the [reference schema](knowledge_graph_schema.md)).
2. Accept **natural-language queries** and retrieve **graph-aware evidence**.
3. Use an LLM to **generate responses grounded in that evidence**, with references to the supporting publications.
4. Support at least one **application** that goes beyond conventional paper search (see [applications](../applications/README.md)).
5. Be **evaluated against conventional retrieval baselines** (see [evaluation](evaluation.md)).

The project is open-ended. Teams choose their application, techniques and research contribution. Possible techniques include graph algorithms, graph ML, multi-hop reasoning, hybrid retrieval, agents, or anything else you can justify.

## Research question

> **Which kinds of questions actually benefit from graph structure, and by how much?**

GraphRAG is not automatically better than vector RAG. It costs more to build, and on many questions it performs no better. A strong project measures *where* graph structure helps (answer quality, evidence retrieval, explainability, latency, cost) and where it does not, using a question set broad enough to tell the difference.

## Minimum working system (required for all teams)

| Component | Requirement |
| --- | --- |
| Graph construction | Ingest the corpus into a knowledge graph with at least `Paper`, `Author`, `Venue` nodes and `AUTHORED`, `PUBLISHED_IN`, `CITES` edges |
| Query processing | Turn a natural-language question into a retrieval plan: entity linking, query decomposition, Cypher generation, or similar |
| Graph-aware retrieval | Retrieve evidence using graph structure (traversal, neighbourhood expansion, path finding, PageRank, community summaries, …) |
| Generation | An LLM answer grounded in the retrieved evidence |
| Attribution | Every answer cites the supporting papers |
| Baselines | At least B0 (closed-book) and B2 (vector RAG) from the [evaluation guide](evaluation.md) |

Beyond this core, choose **one or two** [application directions](../applications/README.md) and go deep.

## Expected outcomes

- A working prototype that integrates a scholarly knowledge graph with a RAG pipeline
- An evaluation set (released with your code) and a quantitative comparison against baselines, broken down by question type
- A report that answers the research question for your chosen application: where did graph structure help, where did it not, and why
- A short demo

## Datasets

The official computer-science corpus snapshot will be provided (coming soon). Until then, start from **DBLP** (bibliographic graph) and **Semantic Scholar** (citations and abstracts). See [dataset/README.md](../dataset/README.md).

The same approach carries over to other domains, such as financial research, policy documents, enterprise knowledge and technical troubleshooting. This project uses scholarly data.
