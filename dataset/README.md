# Dataset

This page tells you where the data comes from and what each source can and cannot give you.

There are two routes:

1. **Official corpus**: a computer-science snapshot prepared for this project (coming soon).
2. **Interim route**: DBLP XML for the bibliographic graph, plus Semantic Scholar for citations and abstracts. Use this to start building now. Everything you write against it carries over.

Throughout the repo, data needs are described as **capabilities** rather than sources, so that your design does not depend on one dataset:

| Capability | What it means | Needed for |
| --- | --- | --- |
| `core` | Papers, authors, venues, years, authorship and publication edges | Everything |
| `citations` | Paper → paper `CITES` edges | Citation paths, PageRank / impact, recommendation, most multi-hop QA |
| `text` | Abstracts (and optionally full text or TLDRs) | Vector RAG baseline, topic/method extraction, any question about *content* |
| `affiliations` | Author → institution | Institution-level questions, collaboration across organisations |
| `derived` | Topics, methods, datasets, tasks extracted by you | Trend analysis, `USES_METHOD` / `HAS_TOPIC` reasoning |

---

## 1. Official corpus (coming soon)

> **Status: not yet released.** This section will be filled in when the snapshot is published. Until then, use the interim route in §2.

The official corpus will be a snapshot of computer-science literature with **papers, venues and authors already linked**. All teams will use the same snapshot, so results can be compared across teams.

| Field | Value |
| --- | --- |
| Release date | _TBA_ |
| Download | _TBA_ |
| Format | _TBA_ |
| Size (compressed / uncompressed) | _TBA_ |
| Scope (fields, years, venues) | _TBA_ |
| Capabilities provided | `core` ✅ · `citations` _TBA_ · `text` _TBA_ · `affiliations` _TBA_ |
| Mapping to the [KG schema](../docs/knowledge_graph_schema.md) | _TBA_ |
| Licence | _TBA_ |

If the snapshot does not include `citations` or `text`, fill the gap from Semantic Scholar as described in §2.2.

---

## 2. Interim route: DBLP + Semantic Scholar

### 2.1 DBLP XML (`core`)

[dblp](https://dblp.org/) is the reference bibliography for computer science. It is curated and has good author disambiguation, and it is released as a monthly XML snapshot under **CC0**.

**Download**

| File | Notes |
| --- | --- |
| [`dblp.xml.gz`](https://dblp.org/xml/dblp.xml.gz) | Current dump, about **1 GB gzipped** (several GB uncompressed). Updated continuously. |
| [`dblp.dtd`](https://dblp.org/xml/dblp.dtd) | **Required.** The XML uses DTD-defined character entities (e.g. `&uuml;`) that do not parse without it. Keep it in the same directory as the XML. |
| [Monthly snapshot releases](https://dblp.org/xml/release/) | Frozen, citable snapshots. **Use one of these, not the rolling file, so your results are reproducible.** Record which release you used in your report. |

```bash
mkdir -p data/raw && cd data/raw
wget https://dblp.org/xml/dblp.xml.gz https://dblp.org/xml/dblp.dtd
```

> Do not commit these files. The provided `.gitignore` already excludes `data/` and `*.xml.gz`. GitHub rejects files over 100 MB.
>
> The dblp **search API** (`dblp.org/search/...`) is behind a bot check and is not suitable for bulk scripted access. Work from the dump.

**What a record looks like.** This is a real record from the October 2026 dump:

```xml
<inproceedings mdate="2025-03-03" key="conf/dac/LiuPS98">
<author>Ying Liu</author>
<author orcid="0000-0002-8605-8240">Lawrence T. Pileggi</author>
<author>Andrzej J. Strojwas</author>
<title><i>ftd</i>: An Exact Frequency to Time Domain Conversion for Reduced Order RLC Interconnect Models.</title>
<pages>469-472</pages>
<year>1998</year>
<crossref>conf/dac/1998</crossref>
<booktitle>DAC</booktitle>
<ee>https://doi.org/10.1145/277044.277174</ee>
<url>db/conf/dac/dac98.html#LiuPS98</url>
</inproceedings>
```

Things to notice:

- **`key`** (`conf/dac/LiuPS98`) is the stable paper ID. Its prefix encodes the venue stream (`conf/dac`, `journals/tods`, …). Semantic Scholar stores this same key as `externalIds.DBLP`, which is your join key (§2.3).
- **Record types** are `article` (journal), `inproceedings` (conference), `proceedings` (the volume, referenced via `crossref`), `incollection`, `book`, `phdthesis`, `mastersthesis`, and `www`. Records with a `homepages/...` key are **author profiles**: they list name variants and can carry `<note type="affiliation">`. Affiliations there are current, not historical, and many are missing.
- **Author names** are already disambiguated. Homonyms get a numeric suffix (`Wei Wang 0001`). Use the full string, suffix included, as the author ID. `orcid` is present only for some authors.
- **Titles can contain inline markup** (`<i>`, `<sub>`, `<sup>`). Take the element's full text, not just `.text`.
- **`ee`** holds links to the electronic edition. When it is a `doi.org` URL, it gives you a DOI.
- **The venue** is `booktitle` (conferences) or `journal` (journals). Abbreviations are not uniform, so normalise venues using the key prefix rather than the display string.

**What DBLP does _not_ give you**

- **No abstracts.** A vector index built on DBLP alone embeds only titles, which is not a meaningful vector-RAG baseline.
- **No citation graph.** A small number of `<cite>` elements exist from a historical project. They are extremely sparse and must not be treated as a citation network.
- **No topics or methods.** These are `derived` (see the [schema](../docs/knowledge_graph_schema.md)).

**Parsing tips**

- Stream the file. It is too large for DOM parsing. Use `lxml.etree.iterparse(..., events=("end",), load_dtd=True)`, filter on the record tags, and call `elem.clear()` after each record (also delete earlier siblings) to keep memory flat.
- The file is declared `ISO-8859-1`. Let the parser handle the encoding and do not decode it yourself.
- Do a first pass that writes compact JSONL/Parquet (one row per paper, plus edge tables). Build everything else from that, not from the XML.

**Cutting a manageable subgraph.** The full dump contains millions of papers. For a 6–8 week project, pick a slice, for example:

- A set of venue key prefixes (e.g. `conf/acl`, `conf/emnlp`, `conf/naacl`, `journals/tacl` for NLP; `conf/kdd`, `conf/www`, `conf/sigir`, `conf/cikm` for data mining / IR)
- A year range (e.g. 2015–2025)

That typically gives tens of thousands of papers, which is large enough for graph structure to matter and small enough to enrich from Semantic Scholar. State your slice precisely in the report.

### 2.2 Semantic Scholar (`citations`, `text`)

The [Semantic Scholar Academic Graph (S2AG)](https://www.semanticscholar.org/product/api) is the reference source for **papers citing papers**. It also provides abstracts, TLDRs, SPECTER2 embeddings and open-access full text. See the paper by Kinney et al. (2023) in the [literature list](../literature/README.md).

**Get an API key first.** Without a key you share one rate-limit pool with every other unauthenticated user, and requests are throttled under load. In practice you will see `429 Too Many Requests` even on single calls. Request a key from the [API page](https://www.semanticscholar.org/product/api#api-key-form). The introductory keyed limit is **1 request/second**, so batch your requests and cache everything locally.

**Graph API**: for enriching a subgraph of up to roughly 10⁵ papers. See the [Graph API docs](https://api.semanticscholar.org/api-docs/graph).

| Endpoint | Use |
| --- | --- |
| `GET /graph/v1/paper/{id}` | One paper. `id` can be an S2 ID or a prefixed external ID such as `DBLP:conf/dac/LiuPS98`, `DOI:10.1145/...` or `ARXIV:2404.16130`. |
| `POST /graph/v1/paper/batch` | **Use this for enrichment.** Up to 500 IDs per call, `fields=` as a query parameter. |
| `GET /graph/v1/paper/{id}/references` | Papers this paper cites (outgoing `CITES`). Paginated. |
| `GET /graph/v1/paper/{id}/citations` | Papers that cite this paper (incoming `CITES`). Paginated. |
| `GET /graph/v1/author/{id}`, `POST /graph/v1/author/batch` | Author metadata (S2 author IDs, which differ from DBLP's) |

Useful `fields`: `externalIds,title,abstract,year,venue,publicationDate,authors,citationCount,referenceCount,influentialCitationCount,tldr,references.paperId,references.externalIds`.

**Datasets API**: for bulk work. Download the corpus-wide files instead of making millions of calls. See the [Datasets API docs](https://api.semanticscholar.org/api-docs/datasets). Downloading the full files requires an API key. Relevant datasets in the current release are `papers`, `abstracts`, `citations`, `authors`, `publication-venues`, `tldrs`, `embeddings-specter_v2` and `s2orc` (full text). Each is around 10–30 GB or more, so filter to your subgraph while streaming. Sample files that don't need a key are listed in the release README.

**Licences.** Check the licence files that ship with each dataset. Abstracts and full text come from third parties and carry their own terms. Do not redistribute S2 data in your repository. Publish scripts that re-fetch it instead.

### 2.3 Joining DBLP and Semantic Scholar

1. **Primary key:** S2 `externalIds.DBLP` equals the DBLP `key`. Look papers up with `POST /paper/batch` using IDs of the form `DBLP:<key>`.
2. **Fallback:** DOI from DBLP `ee` → `DOI:<doi>`.
3. **Last resort:** normalised title + year match. Measure and report the match rate and spot-check false matches.
4. **Citations leave your subgraph.** Most references of a paper point outside your slice. Decide explicitly whether to (a) keep only edges where both ends are in the slice, or (b) add the cited papers as stub nodes (ID, title, year only). Report which you chose. It changes PageRank, path lengths and recommendation results.
5. **Authors:** match DBLP authors to S2 authors only through shared papers, never by name alone.

Report the coverage you achieved, e.g. "92% of DBLP papers matched to S2; 81% have abstracts; median 23 in-slice references".

---

## 3. Other useful sources

These are optional. They are useful as a cross-check, for ground truth, or if you need a capability the main route lacks.

| Source | What it adds | Notes |
| --- | --- | --- |
| [OpenAlex](https://docs.openalex.org/) | Works, authors, **institutions**, venues, concepts and citations in one open (CC0) dataset. No key needed. | Good fallback for `affiliations` and `citations`. Disambiguation is noisier than DBLP's. |
| [AMiner citation datasets](https://www.aminer.cn/citation) | DBLP-derived papers **with citation lists** and abstracts, as a downloadable file | Older snapshots, but quick to start from |
| [STaRK (MAG split)](https://github.com/snap-stanford/stark) | **Ground-truth benchmark** of natural-language queries over a scholarly graph (papers, authors, institutions, fields), each with gold answer nodes | Best off-the-shelf evaluation set for semi-structured, multi-hop retrieval. See [evaluation](../docs/evaluation.md). |
| [LitSearch](https://github.com/princeton-nlp/LitSearch) | Literature-search queries with gold papers (ACL/ICLR papers) | Ground truth for retrieval and recommendation |
| [OGB `ogbn-mag`, `ogbn-arxiv`](https://ogb.stanford.edu/docs/nodeprop/) | Pre-built heterogeneous / citation graphs with fixed splits | For graph-ML components (embeddings, link prediction) |
| [CORE Conference Rankings](https://portal.core.edu.au/conf-ranks/) | A*/A/B/C ranks for CS conferences | External reference for venue ranking (direction 2) |
