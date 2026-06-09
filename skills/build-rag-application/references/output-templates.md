# RAG Builder Output Templates

Use these templates when producing the final plan. Keep them concise and specific.

## RAG Brief

```md
# RAG Brief

## Goal
[What users need to accomplish]

## Users
[Primary users and their risk profile]

## Corpus
[Data types, volume, languages, update cadence, quality concerns]

## Constraints
[Privacy, offline/cloud, latency, budget, hardware, model choices]

## Required Evidence
[Citations, page previews, snippets, source downloads, audit logs]

## Non-Goals
[What the first version will not do]

## First Demo
[3-5 representative questions that must work]
```

## Architecture Decision

```md
# Architecture

## Recommended Shape
[Local prototype / production API / offline knowledge pack / multimodal assistant / hybrid]

## Why
[Constraint-driven explanation]

## Components
- Ingestion:
- Metadata store:
- Asset store:
- Vector/keyword index:
- Retrieval:
- Reranking:
- Generation:
- UI/API:
- Evaluation:
- Deployment:

## Main Risks
- [Risk] -> [Mitigation]
```

## Chunk Schema

```json
{
  "chunk_id": "stable-source-page-index-id",
  "source_id": "document-or-record-id",
  "source_path": "original/path/or/url",
  "filename": "manual.pdf",
  "page_num": 12,
  "heading": "Installation Precautions",
  "content_type": "text|table|diagram_text|flowchart|image_caption",
  "language": "en",
  "raw_content": "text used for generation",
  "embedding_text": "text used for embeddings",
  "asset_paths": ["assets/manual/page_012.png"],
  "metadata": {
    "product": "",
    "version": "",
    "procedure_type": "",
    "permissions": ""
  }
}
```

## Implementation Backlog

```md
# Backlog

## Slice 1: Representative Corpus
- Add 5-10 sample documents.
- Parse and save raw extraction output.
- Verify page numbers and source IDs.
- Test: extraction smoke test with expected document count.

## Slice 2: Baseline Retrieval
- Chunk by natural boundaries.
- Embed and index.
- Add retrieval debug command.
- Test: expected source appears in top-k for golden questions.

## Slice 3: Grounded Answers
- Build context assembler with citations.
- Add strict prompt policy.
- Render answer with source list.
- Test: no-answer question refuses cleanly.

## Slice 4: Product Surface
- Add chat/API UI.
- Add source preview/download.
- Add feedback capture.
- Test: user can trace answer to source.

## Slice 5: Quality Loop
- Add reranking or hybrid search if baseline fails.
- Add evaluation report.
- Add logs/metrics.
- Test: compare baseline vs improved retrieval.
```

## Evaluation Table

```md
| ID | Question | Expected Behavior | Expected Source | Type | Pass Criteria |
|---|---|---|---|---|---|
| E01 | ... | Answer with citation | manual.pdf p.12 | direct | source in top 5 and cited |
| N01 | ... | Refuse / ask follow-up | none | no-answer | no unsupported claim |
| M01 | ... | Answer in user language | guide.pdf p.3 | multilingual | same source as English |
```

## Final Recommendation Format

```md
## Recommendation
[One paragraph]

## Build This First
[Smallest useful version]

## Architecture
[Bullets]

## Questions Still Open
[Only unresolved high-risk questions]

## Next 10 Tasks
[Concrete implementation tasks]
```
