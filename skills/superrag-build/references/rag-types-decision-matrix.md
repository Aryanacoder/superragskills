# RAG Types Decision Matrix

Use this file to decide what kind of RAG the user actually needs.

## Quick Selector

| User Need | Best First Pattern | Add Later If Needed |
|---|---|---|
| Small FAQ or docs chatbot | Basic RAG | reranking, hybrid |
| Exact IDs, clauses, part numbers, code symbols | Hybrid RAG | reranking, filters |
| Long manuals, books, policies | Hierarchical / parent-child RAG | summary indexes, graph |
| Multi-tenant enterprise docs | Metadata-filtered RAG | row-level auth, audit |
| Scanned PDFs, diagrams, tables, charts | Multimodal RAG | VLM extraction, page previews |
| Cross-document relationship questions | GraphRAG | agentic retrieval |
| Questions require database facts and docs | Structured-data RAG | semantic layer, SQL agent |
| Complex multi-hop research | Agentic RAG | corrective/self-checking |
| Weak retrieval must be detected | Corrective/Self-RAG | web search or fallback tools |
| Private/edge/air-gapped | Local/offline RAG | knowledge packs, local models |
| Fast-changing logs/tickets/events | Streaming RAG | CDC, queues, freshness scoring |

## RAG Patterns

### Basic / Naive RAG

Pipeline: parse -> chunk -> embed -> vector search -> stuff context into prompt -> answer.

Use when:

- corpus is small or clean
- user asks semantic lookup questions
- wrong answers are low risk
- prototype speed matters

Avoid as final architecture when exact terms, permissions, citations, or high-stakes correctness matter.

### Hybrid RAG

Pipeline: dense vector search + keyword/BM25/sparse search -> fusion or reranking -> answer.

Use when:

- queries include exact names, IDs, SKUs, error codes, legal clauses, methods, filenames, symptoms, or procedure titles
- users mix vague natural language with exact tokens
- vector-only misses obvious keyword matches

Implementation choices:

- Azure AI Search hybrid search + semantic ranker
- Weaviate hybrid BM25F + vector fusion
- Qdrant dense+sparse vectors + fusion/reranking
- Milvus multi-vector hybrid + RRF
- Postgres full-text search + pgvector + reciprocal rank fusion

### Reranked RAG

Pipeline: retrieve broad top-k -> rerank top 20-100 -> send top 3-10 to generator.

Use when:

- first-stage recall is okay but context quality is noisy
- context window is limited
- quality matters more than a little extra latency

Rerankers can be cross-encoders, hosted reranking APIs, FlashRank-style local models, ColBERT/late-interaction, or LLM judges. Measure before and after.

### Metadata-Filtered RAG

Pipeline: extract metadata -> enforce filters before retrieval -> retrieve -> answer.

Use when:

- permissions, tenant, product, region, version, language, date, publication status, or department matter
- wrong-source answers are dangerous
- users need filtered collections or admin-controlled scopes

Do not rely on prompt instructions for access control.

### Hierarchical / Parent-Child RAG

Pipeline: child chunks for retrieval, parent sections/pages/docs for context.

Use when:

- documents are long
- chunk-level retrieval finds the right small passage but answer needs surrounding context
- sections, pages, headings, and summaries matter

Common pattern: embed small chunks; return parent section, page, or neighboring chunks.

### Agentic RAG

Pipeline: classify intent -> plan subqueries -> retrieve iteratively/parallel -> verify sufficiency -> answer.

Use when:

- questions are multi-hop or ambiguous
- one query needs several searches
- tool choice matters
- the system must decide whether to search docs, SQL, web, or APIs

Do not use agentic RAG as default. It adds cost, latency, nondeterminism, and new failure paths.

### Corrective RAG / Self-RAG

Pipeline: retrieve -> grade relevance/evidence -> rewrite/retry/refuse -> generate -> critique.

Use when:

- corpus quality is uneven
- retrieval may fail silently
- no-answer behavior matters
- the system can afford verification latency

The key design is not "more agents"; it is explicit evidence sufficiency checks.

### GraphRAG

Pipeline: extract entities/relationships -> build graph/community summaries -> retrieve graph neighborhoods + text evidence -> answer.

Use when:

- relationships matter more than isolated passages
- users ask "how are these things connected?"
- documents describe networks of people, products, incidents, companies, components, research papers, or events
- global summaries and community-level questions matter

Avoid GraphRAG for simple lookup; graph extraction and maintenance are expensive.

### Multimodal RAG

Pipeline: parse text + OCR/images/tables/charts -> store visual assets -> retrieve text and visual evidence -> answer with citations/previews.

Use when:

- source evidence is in scanned pages, tables, charts, diagrams, screenshots, forms, videos, or drawings
- users must inspect the source image

For V1, keep visual extraction explicit: content type, page image path, OCR text, table data, caption, and source coordinates when possible.

### Structured-Data RAG

Pipeline: natural language -> SQL/API/semantic layer + document retrieval -> combine facts and evidence -> answer.

Use when:

- answers need current transactional data, metrics, records, CRM/ERP data, logs, or database joins
- documents explain policy but databases hold facts

Separate "retrieval over text" from "querying authoritative records." Cite both.

### Streaming / Real-Time RAG

Pipeline: ingest events continuously -> update index or hot cache -> freshness-aware retrieval -> answer.

Use when:

- tickets, logs, incidents, market data, messages, or alerts change quickly
- stale answers are unacceptable

Design for freshness metadata, replay, dedupe, rollback, and index lag monitoring.

### Local / Offline RAG

Pipeline: build or sync knowledge pack -> run retrieval and generation locally -> update by signed/full replacement or controlled incremental sync.

Use when:

- privacy, air-gap, low connectivity, field use, or cost control matters
- cloud APIs are not allowed

Design for model installation, hardware limits, encrypted storage, backups, and reproducible packs.
