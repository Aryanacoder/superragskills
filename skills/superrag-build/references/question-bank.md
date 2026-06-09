# RAG Builder Question Bank

Use this file when the user needs a thorough discovery interview. Ask in batches; never dump the entire bank unless the user asks for a worksheet.

## 1. Goal And Users

- What job should the RAG app help users complete?
- Who are the primary users: internal team, customers, field staff, analysts, engineers, support, legal, medical, finance, students?
- What is the cost of a wrong answer: inconvenience, lost time, money, safety, compliance, legal exposure?
- Should the system answer, summarize, compare, troubleshoot, draft, classify, extract, recommend, or guide procedures?
- What questions should it never answer?
- What should it do when evidence is weak: refuse, ask a follow-up, give closest references, or escalate?
- What is the first demo question that must work?

## 2. Data And Corpus

- What source types exist: PDF, DOCX, PPTX, XLSX, CSV, HTML, Markdown, email, database rows, tickets, code, images, audio, video?
- Are PDFs native text, scanned, mixed, password-protected, rotated, handwritten, or full of tables/diagrams?
- Which languages appear in documents and user questions?
- How many documents, pages, tokens, images, rows, or records exist now?
- How often does data change: never, monthly, daily, real-time?
- Is there a single source of truth, or multiple conflicting versions?
- Do documents have permissions, tenants, departments, products, regions, versions, dates, or approval status?
- Are there duplicate, obsolete, draft, or low-quality documents?
- Can the app store extracted text, embeddings, page images, and source snippets?

## 3. Evidence And Output

- Must answers include citations, page numbers, file names, direct snippets, screenshots, table references, or downloadable source files?
- Should the app quote exact text or paraphrase?
- Should it show confidence, evidence gaps, or alternative interpretations?
- Are users expected to verify source pages before acting?
- Should answers be short, procedural, table-based, JSON, report style, or chatty?
- Should non-English users receive answers in their language, English, or both?

## 4. Ingestion

- Where will ingestion run: user laptop, server, CI job, cloud worker, offline developer machine?
- Which parsers are acceptable: PyMuPDF, Docling, Unstructured, Tika, Azure/AWS/GCP OCR, Tesseract, EasyOCR, custom ETL?
- Do tables need structure preserved?
- Do diagrams/flowcharts/images need OCR, captions, or visual-language-model extraction?
- How should deduplication work: file hash, page hash, text signature, semantic similarity?
- Should indexing be incremental, full replacement, or both?
- What manifest should be saved: source hash, extraction method, chunk count, model names, timestamp, schema version?

## 5. Chunking And Metadata

- What is the natural unit of meaning: section, page, heading, procedure, paragraph, row, ticket, code symbol, clause?
- Are there step-by-step procedures that must not be split badly?
- What metadata is needed for filtering or citations: source path, file name, page, heading, product, equipment, version, language, content type, permissions?
- Should chunks include raw text separately from embedding text?
- How much overlap is safe before duplicates pollute retrieval?
- Should images, cropped pages, and tables be stored as linked assets?

## 6. Retrieval

- Do questions rely on semantic meaning, exact keywords, IDs, part numbers, dates, symptoms, or filenames?
- Is metadata filtering required before vector search?
- Is hybrid search needed: dense vectors plus BM25/sparse keywords?
- How broad should first-stage retrieval be, and how many chunks should survive reranking?
- Which reranker is allowed: local cross-encoder, FlashRank, Cohere, Jina, bge-reranker, LLM judge, none?
- Should queries be rewritten, translated, decomposed, or expanded with synonyms?
- Should retrieval return source pages even when generation refuses?

## 7. Generation

- Which model family is preferred: local Ollama, OpenAI, Anthropic, Gemini, Azure OpenAI, Bedrock, vLLM?
- What are latency and cost limits?
- Should generation stream tokens?
- What context budget is available?
- What system prompt rules are non-negotiable?
- Should the app cite per sentence, per paragraph, or in a source list?
- Should answer generation be deterministic or creative?

## 8. UI And Product

- Is the surface chat, API, Slack/Teams bot, web app, Chainlit, Streamlit, CLI, browser extension, mobile, or embedded workflow?
- Does the user need source previews, page image carousel, PDF downloads, filters, collection selection, admin dashboard, feedback buttons, or logs?
- Should users upload documents themselves?
- Are there roles: end user, reviewer, admin, ingestion operator?
- What needs to be visible for debugging: retrieved chunks, scores, rerank reasons, prompt context, model timings?

## 9. Evaluation

- What are 20-50 real questions with expected sources?
- Which questions should have no answer?
- Which questions are ambiguous and should trigger a clarification?
- Which questions test tables, diagrams, multilingual phrasing, outdated docs, and exact values?
- What metrics matter: recall@k, MRR, citation accuracy, faithfulness, answer completeness, latency, cost?
- Who reviews failures and approves changes?

## 10. Deployment And Operations

- Will the app run local/offline, on-prem, cloud, hybrid, or air-gapped?
- What hardware exists at runtime: CPU only, GPU, RAM, disk, OS, Docker availability?
- How are models installed and updated?
- How are indexes backed up, versioned, rolled back, and migrated?
- How are secrets, PII, permissions, logs, and audit trails handled?
- What monitoring is needed: ingestion errors, retrieval misses, answer refusals, user feedback, latency, token usage?
