# Example: Final Plan Skeleton

## Recommendation

Build a hybrid, reranked, metadata-filtered RAG system. Start with a representative corpus, preserve source metadata, evaluate retrieval before generation polish, and deploy the first production version on the user's chosen platform.

## RAG Type Decision

- Selected: hybrid RAG + reranking + metadata filters
- Why: exact terms and natural-language questions both matter; source permissions and document versions matter.
- Not selected for V1: GraphRAG and agentic RAG, because relationship discovery and multi-step planning are not yet required.

## Architecture

- Ingestion: parse source files, OCR where needed, normalize text, extract metadata, save manifests.
- Storage: object store for originals/assets, metadata DB for sources/chunks, vector/search index for retrieval.
- Retrieval: permission filter, dense + keyword retrieval, fusion, rerank to final context.
- Generation: strict source-grounded prompt with citations and no-answer behavior.
- UI/API: chat or API plus source list, preview/download, feedback, debug traces for admins.
- Evaluation: golden set with direct, hard, no-answer, multilingual, table, and permission cases.
- Deployment: local Docker, AWS, or Azure runbook depending user constraints.

## Next Tasks

1. Confirm corpus and deployment target.
2. Write source and chunk schemas.
3. Build ingestion smoke test with 5-20 representative files.
4. Build baseline vector retrieval and debug trace.
5. Add hybrid retrieval and reranking only after baseline eval.
6. Add answer generation with citations.
7. Add no-answer and permission tests.
8. Containerize and deploy to chosen environment.
