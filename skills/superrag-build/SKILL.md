---
name: superrag-build
description: End-to-end skill for coding agents to design, build, evaluate, and deploy any retrieval-augmented generation system, including naive RAG, advanced/hybrid RAG, agentic RAG, GraphRAG, multimodal RAG, structured-data RAG, local/offline RAG, AWS Bedrock RAG, Azure AI Search RAG, and self-hosted production RAG. Use when the user wants to build a RAG app, choose the right RAG architecture, improve retrieval quality, add citations, deploy to AWS/Azure/local servers, or turn rough data and goals into a production-ready implementation plan.
---

# superRAG-build

## Mission

Be the RAG architect, interviewer, implementation planner, and deployment guide. Turn an unclear RAG idea into the simplest architecture that can satisfy the user's data, risk, quality, budget, and deployment constraints.

Do not jump straight to code for broad RAG requests. First classify the RAG type, choose the stack, define the evaluation path, and identify deployment constraints. If the user asks for a tiny prototype, ask only the missing high-risk questions and build a narrow vertical slice.

## First Response

Ask the first batch of questions, then adapt:

1. What job should this RAG app help users complete?
2. Who are the users, and what is the cost of a wrong answer?
3. What data exists: file types, volume, languages, scans, images, tables, databases, websites, code, APIs?
4. Does every answer need citations, page previews, exact quotes, source downloads, or audit logs?
5. Is this local/offline, cloud, hybrid, multi-tenant, or enterprise/private?
6. Preferred stack: AWS, Azure, OpenAI-hosted, open source, LangChain, LlamaIndex, custom, no preference?
7. What is the first demo query that must work?

Then say which RAG families are likely candidates and what you need next. Ask questions in batches; do not dump the whole questionnaire unless the user asks for a worksheet. Use [references/question-bank.md](references/question-bank.md) for exhaustive discovery.

## Architecture Selection

Use [references/rag-types-decision-matrix.md](references/rag-types-decision-matrix.md) before choosing tools.

Default choices:

- **Naive/basic RAG**: small corpus, low risk, natural-language lookup, quick prototype.
- **Hybrid RAG**: exact terms, IDs, part numbers, policy clauses, support tickets, code, procedures, or mixed vague/specific queries.
- **Reranked RAG**: quality matters and first-stage retrieval returns too many candidates.
- **Metadata-filtered RAG**: tenants, permissions, product/version filters, regions, dates, languages, document status.
- **Hierarchical/parent-child RAG**: long PDFs, books, policy docs, manuals, multi-section reports.
- **Agentic RAG**: multi-hop questions, query planning, iterative retrieval, ambiguity, tool use, cross-document synthesis.
- **Corrective/Self-RAG**: retrieval may be weak and the system must evaluate, retry, or refuse.
- **GraphRAG**: entity relationships, communities, investigations, knowledge discovery, cross-document relationship queries.
- **Multimodal RAG**: images, diagrams, scans, charts, screenshots, video frames, CAD/flowcharts, visual tables.
- **Structured-data RAG**: SQL/BI/metrics/doc hybrids where the answer needs database queries plus text evidence.
- **Streaming/real-time RAG**: fast-changing data, events, logs, tickets, CDC, queues.
- **Local/offline RAG**: air-gapped, privacy, edge machines, field laptops, classified or regulated data.

If two options compete, pick the simpler one for V1 and design extension points for V2.

## Build Flow

Use [references/build-from-scratch.md](references/build-from-scratch.md) for detailed implementation steps.

1. Define the product contract: users, answers, refusals, citations, latency, update cadence.
2. Build a golden evaluation set before optimizing: easy, hard, no-answer, ambiguous, multilingual, table/diagram cases.
3. Ingest 5-20 representative documents or records.
4. Preserve source metadata and stable IDs before embeddings.
5. Chunk by natural structure: headings, pages, procedures, clauses, rows, code symbols, tickets.
6. Build baseline retrieval with debug traces.
7. Add citations and source inspection before answer polish.
8. Add hybrid search, filters, reranking, query rewriting, or agentic retrieval only when eval says baseline fails.
9. Add deployment, auth, observability, backups, and update pipelines.
10. Run the evaluation suite and ship only when retrieval and answer faithfulness meet the stated quality bar.

## Platform Selection

Use [references/provider-stack-matrix.md](references/provider-stack-matrix.md) and [references/deployment-playbooks.md](references/deployment-playbooks.md).

Fast defaults:

- **OpenAI-hosted file search**: fastest managed prototype when hosted file search is acceptable.
- **Azure AI Search + Azure OpenAI**: strong enterprise Azure choice, hybrid/vector/semantic ranking, managed identity, App Service or Container Apps deployment.
- **AWS Bedrock Knowledge Bases**: strong AWS managed RAG choice, S3 ingestion, OpenSearch Serverless/Aurora/MongoDB vector stores, Bedrock models/agents.
- **Qdrant/Weaviate/Milvus + FastAPI + LangChain/LlamaIndex**: open-source production control.
- **Postgres + pgvector + full-text search**: transactional app data, joins, permissions, simpler ops.
- **FAISS/LanceDB/Chroma**: local prototype, desktop app, notebook, small read-only knowledge pack.
- **Docker Compose with Qdrant/Ollama or vLLM**: private/local server.
- **Kubernetes/Helm**: multi-service production with GPUs, autoscaling, monitoring, and controlled rollouts.

## Required Deliverables

For design tasks, produce:

1. **RAG brief**: goal, users, risk, corpus, constraints, non-goals.
2. **RAG type decision**: selected RAG pattern and rejected alternatives.
3. **Architecture**: ingestion, metadata, assets, retrieval, generation, UI/API, eval, deployment.
4. **Data contract**: source schema, chunk schema, citation schema, permission model.
5. **Build plan**: vertical slices with tests and validation commands.
6. **Deployment plan**: local/AWS/Azure/server runbook, environment variables, secrets, scaling, backups.
7. **Evaluation plan**: golden set, metrics, tracing, failure review.

Use [references/output-templates.md](references/output-templates.md) for ready-to-fill formats.

## Implementation Guardrails

- Retrieval quality comes before prompt cleverness.
- Treat permissions as retrieval filters, not UI-only checks.
- Preserve source IDs, page/row/time offsets, and extraction method.
- Log retrieval candidates, scores, filters, rerank results, prompt context IDs, model versions, latency, token cost, and refusal reasons.
- Never let feedback automatically rewrite production prompts, indexes, or answer policy without review.
- For high-stakes domains, cite every operational, legal, financial, medical, safety, or technical claim.
- If evidence is weak, ask a follow-up, provide closest references, or refuse.
- For agentic RAG, guard against cascading hallucination: validate each retrieval/reasoning stage before letting it affect final answers.

## When Coding

Inspect the existing repo first:

- ingestion/parsing code
- chunk schema and metadata
- vector/keyword store
- retrieval and reranking
- prompt/context assembly
- UI/API
- auth/permissions
- evals/tests/logs
- deployment files

Then implement the smallest end-to-end slice. Add abstractions only when they reduce real complexity or match the local codebase.

## Reference Map

- RAG type selection: [references/rag-types-decision-matrix.md](references/rag-types-decision-matrix.md)
- Full questionnaire: [references/question-bank.md](references/question-bank.md)
- Build from scratch: [references/build-from-scratch.md](references/build-from-scratch.md)
- Architecture details: [references/architecture-playbook.md](references/architecture-playbook.md)
- Provider/platform choice: [references/provider-stack-matrix.md](references/provider-stack-matrix.md)
- AWS/Azure/local deployment: [references/deployment-playbooks.md](references/deployment-playbooks.md)
- Evaluation and observability: [references/evaluation-observability.md](references/evaluation-observability.md)
- Security and governance: [references/security-governance.md](references/security-governance.md)
- Public research/doc sources: [references/research-sources.md](references/research-sources.md)
