# SuperRAG

The complete agent skill pack for designing, building, evaluating, and deploying retrieval-augmented generation applications.

`SuperRAG` helps coding agents guide a user from "I want a RAG app" to a production-ready plan and implementation. It covers basic RAG, hybrid RAG, reranked RAG, metadata-filtered RAG, hierarchical RAG, GraphRAG, agentic RAG, corrective/self-RAG, multimodal RAG, structured-data RAG, local/offline RAG, and cloud deployments on AWS and Azure.

## What It Includes

- A primary agent skill: `skills/superrag-build/SKILL.md`
- RAG type decision matrix
- Full discovery questionnaire
- From-scratch build playbook
- Provider and stack matrix
- AWS, Azure, local server, Docker, and Kubernetes deployment playbooks
- Evaluation and observability guide
- Security and governance guide
- Output templates and examples for coding agents

## Layout

```text
superRAG-build/
  skills/
    superrag-build/
      SKILL.md
      agents/openai.yaml
      references/
      examples/
  .claude-plugin/
  .cursor/rules/
  AGENTS.md
  CLAUDE.md
```

## Install Manually

Claude Code:

```bash
cp -r skills/superrag-build ~/.claude/skills/superrag-build
```

Codex:

```bash
mkdir -p .agents/skills
cp -r skills/superrag-build .agents/skills/superrag-build
```

Cursor:

```bash
mkdir -p .cursor/skills
cp -r skills/superrag-build .cursor/skills/superrag-build
```

For Cursor rule-based fallback, keep `.cursor/rules/superrag-build.mdc` in the repo.

## Example Prompts

```text
Use superRAG-build. Interview me and decide which RAG architecture I need.
```

```text
Use superRAG-build to design and implement a production RAG app for PDFs, tables, and internal tickets, deployable on Azure.
```

```text
Use superRAG-build to compare AWS Bedrock Knowledge Bases, Azure AI Search, and a self-hosted Qdrant stack for my requirements.
```

## Privacy

This repository is built from public documentation, research, and generalized engineering lessons. It should not include private application code, private customer data, internal file paths, or proprietary corpus details.
