# RAG Architecture Playbook

Use this reference after the interview to choose a practical architecture. Prefer boring, inspectable systems before complex agentic layers.

## Common Architecture Shapes

### Local Prototype

Best when the user needs a fast demo, small corpus, private data, or no infrastructure.

- Parser: PyMuPDF, pypdf, Docling, pandas/openpyxl, BeautifulSoup.
- Store: local files plus SQLite metadata.
- Index: FAISS, LanceDB, Chroma, or SQLite FTS plus vectors.
- Runtime: Streamlit, Chainlit, FastAPI, CLI, or notebook.
- Risk: weak multi-user ops, manual updates, permission modeling often deferred.

### Production API

Best when multiple clients, auth, monitoring, and repeatable updates matter.

- Ingestion workers produce normalized documents, chunks, assets, and manifests.
- Metadata database stores source and chunk records.
- Vector store supports filters and index versioning.
- Retrieval service exposes debug traces and source references.
- Generation service applies answer policy and logs evidence.
- UI consumes citations and source previews from stable IDs.

### Offline Knowledge Pack

Best when indexing is heavy but runtime must be local, private, or disconnected.

- Build indexes on a stronger machine.
- Emit a zip or directory containing vector index, metadata DB/JSONL, page images/assets, and manifest.
- Include schema version, embedding model, chunking settings, source hashes, timestamp, and checksum.
- Runtime loads the pack read-only and never mutates live retrieval without supervised update.
- Updates are full replacement unless incremental sync is a hard requirement.

### Multimodal Technical Assistant

Best when scanned PDFs, diagrams, flowcharts, screenshots, or tables carry important meaning.

- Extract native text first.
- Run OCR for image-only pages and embedded images.
- Preserve page images and cropped assets for preview.
- Emit content types: text, table, diagram_text, flowchart, image_caption, figure.
- For flowcharts, convert visual structure into graph-like text when possible.
- Retrieval should boost content types that match query intent.

## Component Choices

### Parsing

- Use native text extraction for speed when PDFs are clean.
- Use structure-aware parsers when headings, tables, and layout matter.
- Use OCR fallback when native text is sparse or empty.
- Store extraction method in metadata so failures are diagnosable.
- Save raw extracted text before cleanup.

### Chunking

- Prefer semantic boundaries: headings, procedures, pages, clauses, rows, tickets, code symbols.
- Keep procedures intact when possible.
- Use overlap only to protect context, not to hide bad boundaries.
- Store `raw_content` for generation and `embedding_text` for vectorization when cleaning differs.
- Store chunk indexes and stable IDs for citation mapping.

### Metadata

Minimum useful fields:

- `chunk_id`
- `source_id`
- `source_path`
- `filename`
- `page_num`
- `heading`
- `content_type`
- `language`
- `created_at` or `source_modified_at`
- `extraction_method`
- `chunk_index`
- `permissions` or `tenant_id` when applicable

Domain-specific fields should mirror how users ask questions: product, equipment, account, region, procedure type, version, policy area, case type, part number, project, or document status.

### Embeddings

- Choose embedding model based on language, domain, cost, latency, and deployment.
- Multilingual corpora need multilingual embeddings or normalized translated queries.
- Re-embed everything when changing embedding model or meaningfully changing chunk text.
- Record embedding model and dimensionality in the manifest.

### Retrieval

Use a two-stage retrieval pattern for most serious apps:

1. Broad candidate retrieval with vectors, keyword search, or hybrid search.
2. Rerank candidates with a cross-encoder, fast reranker, or LLM judge.

Add metadata filters before retrieval when permissions, product, version, language, or document status matter. Add query rewriting only when baseline retrieval traces show it helps.

### Generation

The prompt should specify:

- Answer only from provided sources.
- Cite claims.
- Preserve procedure order and numeric values.
- State missing evidence.
- Ask follow-up questions when the user request is underspecified.
- Refuse unsupported, unsafe, or out-of-scope answers.

For multilingual apps, prefer retrieval over a normalized query while preserving original user intent and technical terms. Translate answers after generation only if it does not break citations or safety wording.

### Evaluation

Evaluate retrieval before answer quality. A good generator cannot recover missing evidence.

Build a golden set with:

- easy direct lookup questions
- hard synthesis questions
- exact value/table questions
- no-answer questions
- ambiguous questions
- multilingual paraphrases
- OCR/diagram questions if relevant
- outdated or conflicting source questions

Track:

- expected source in top-k
- expected source after rerank
- citation accuracy
- unsupported claim count
- answer completeness
- latency and cost

### Operational Guardrails

- Do not let feedback directly rewrite the index, prompt, or answer policy.
- Keep indexes versioned and rollbackable.
- Log source IDs, chunk IDs, scores, rerank scores, prompt context IDs, model names, and refusal reasons.
- Treat permissions as a retrieval filter, not a UI-only concern.
- Keep source previews available for users who must verify before action.
