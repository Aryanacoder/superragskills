# Public Research And Documentation Sources

This skill pack should be updated from public sources, not private codebases. Useful source categories:

## Foundational And Research

- Retrieval-Augmented Generation introduced the pattern of combining parametric generation with non-parametric retrieval.
- Self-RAG adds retrieval/generation critique through self-reflection.
- Corrective RAG adds retrieval quality evaluation and corrective actions before generation.
- Recent agentic RAG surveys classify systems by planning, tool use, memory, iterative retrieval, and evaluation.
- Recent chunking and hybrid retrieval papers reinforce that retrieval quality depends heavily on chunking, search method, and reranking.

## Official Documentation To Prefer

- OpenAI File Search and vector store docs for hosted file-search RAG.
- AWS Bedrock Knowledge Bases docs for AWS-managed ingestion, retrieval, hybrid search, and RetrieveAndGenerate.
- Azure AI Search docs for vector, hybrid, semantic ranker, multimodal search, classic RAG, and agentic retrieval.
- LangChain and LangGraph docs for retrieval chains and agentic workflows.
- LlamaIndex docs for ingestion, indexing, query engines, and evaluation.
- Qdrant docs for hybrid search, reranking, filtering, and deployment.
- Weaviate docs for hybrid BM25F/vector search and generative search.
- Milvus/Zilliz docs for multi-vector hybrid search and reranking.
- Pinecone docs for reranking and relevance optimization.
- Docker docs for local RAG containers with Ollama/Qdrant.
- NVIDIA RAG Blueprint docs for self-hosted Docker/Kubernetes RAG deployments.

## Source URLs Checked During 2026-06 Refresh

- https://platform.openai.com/docs/guides/tools-file-search
- https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html
- https://docs.aws.amazon.com/bedrock/latest/userguide/kb-test-config.html
- https://learn.microsoft.com/en-us/azure/search/hybrid-search-overview
- https://learn.microsoft.com/en-us/azure/search/hybrid-search-ranking
- https://learn.microsoft.com/en-us/azure/search/search-agentic-retrieval-concept
- https://learn.microsoft.com/en-us/azure/app-service/tutorial-ai-openai-search-nodejs
- https://docs.langchain.com/oss/python/langchain/retrieval
- https://docs.langchain.com/oss/python/langchain/rag
- https://llamaindex.openml.io/python/framework/understanding/rag/loading/
- https://llamaindex.openml.io/python/framework/module_guides/evaluating/
- https://qdrant.tech/documentation/advanced-tutorials/reranking-hybrid-search/
- https://docs.weaviate.io/weaviate/search/hybrid
- https://milvus.io/docs/hybridsearch.md
- https://docs.pinecone.io/guides/optimize/increase-relevance
- https://docs.docker.com/guides/rag-ollama/develop/
- https://docs.nvidia.com/rag/latest/deploy-docker-self-hosted.html
