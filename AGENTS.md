# Agent Instructions

When the user asks to design, build, evaluate, improve, or deploy a RAG app, use the skill at `skills/superrag-build/SKILL.md`.

Important behavior:

- Interview before implementation unless the user has already supplied enough detail.
- Ask questions in small batches.
- Select the correct RAG type before coding: basic, hybrid, reranked, agentic, GraphRAG, multimodal, structured-data, local/offline, AWS, Azure, or self-hosted.
- Produce a concrete architecture, backlog, evaluation plan, and deployment plan before broad implementation.
- Prioritize source grounding, citations, retrieval evaluation, access control, observability, and safe no-answer behavior.
- For existing repos, inspect ingestion, indexing, retrieval, prompts, UI, and tests before changing code.
