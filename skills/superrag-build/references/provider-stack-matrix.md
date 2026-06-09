# Provider And Stack Matrix

Use this to choose tooling.

## Managed RAG Platforms

| Platform | Best For | Watchouts |
|---|---|---|
| OpenAI File Search / vector stores | fastest hosted prototype, simple app-managed files | less control over custom retrieval internals than self-built stacks |
| AWS Bedrock Knowledge Bases | AWS-native S3 ingestion, managed embeddings/retrieval, Bedrock models/agents | region/model/store feature availability varies |
| Azure AI Search + Azure OpenAI | enterprise Azure, hybrid search, semantic ranker, integrated vectorization, managed identity | Azure architecture/roles can be verbose |
| Google Vertex AI / AlloyDB / Matching Engine | GCP-native apps and data | choose service mix carefully |
| Databricks Mosaic AI Vector Search | lakehouse data, governance, enterprise analytics | best when data already lives in Databricks |

## Vector / Search Stores

| Store | Use When | Avoid When |
|---|---|---|
| FAISS | local prototype, read-only pack, small/medium in-memory retrieval | multi-user ops, filtering, updates, persistence management matter |
| Chroma | local/dev apps and prototypes | large enterprise ops or strict SLA required |
| LanceDB | local/serverless file-backed vector data, multimodal workflows | complex distributed search ops needed |
| Qdrant | open-source production vector search, filters, hybrid dense/sparse, self-host/cloud | need SQL joins as primary feature |
| Weaviate | built-in hybrid BM25F/vector, modules, generative search | want minimal moving parts inside existing Postgres app |
| Milvus/Zilliz | high-scale vector search, multi-vector, hybrid, Kubernetes | small apps that need simple ops |
| Pinecone | managed vector search/reranking with low ops | strict self-hosting or database joins needed |
| Postgres + pgvector | app data and permissions live in Postgres, joins/transactions matter | very large vector scale or specialized ANN ops dominate |
| Azure AI Search | Azure enterprise search, hybrid/vector/semantic/multimodal | non-Azure stack or self-host requirement |
| OpenSearch/Elasticsearch | existing search infra, BM25 + vector hybrid, logs/docs | pure vector simplicity needed |

## Frameworks

| Framework | Best For | Watchouts |
|---|---|---|
| LangChain + LangGraph | agentic workflows, tool orchestration, integrations | abstraction can hide retrieval details; keep traces |
| LlamaIndex | ingestion/index abstractions, query engines, evaluation | still define your own schemas and evals |
| Haystack | production search/RAG pipelines, open-source components | ecosystem choices differ from LangChain |
| DSPy | optimizing prompts/retrieval programs | adds another evaluation/optimization layer |
| Custom FastAPI service | maximum control, clearer production contracts | more code to write |

## Model Choices

| Need | Recommendation |
|---|---|
| highest quality hosted generation | OpenAI / Azure OpenAI / Anthropic / Gemini depending policy |
| AWS-native | Bedrock foundation models and Titan/Cohere embeddings |
| local CPU/GPU prototype | Ollama for simplicity |
| local production GPU serving | vLLM/TGI/Triton/NVIDIA stack |
| multilingual retrieval | multilingual embedding model, language-aware fields, or query translation |
| reranking | cross-encoder, bge-reranker, Cohere/Jina/Pinecone rerank, FlashRank, ColBERT-style late interaction |

## Default Stack Recipes

### Fast Local Prototype

- Python
- LlamaIndex or LangChain
- PyMuPDF/Docling
- FAISS/Chroma/LanceDB
- OpenAI or local Ollama
- Streamlit/Chainlit/FastAPI

### Production Open Source

- FastAPI backend
- worker queue for ingestion
- Postgres metadata
- Qdrant/Weaviate/Milvus vector store
- object storage for files/assets
- reranker
- tracing/metrics
- Docker/Kubernetes

### Enterprise Azure

- Azure Blob Storage
- Azure AI Search
- Azure OpenAI / Foundry model
- App Service or Container Apps
- Managed Identity and Key Vault
- Application Insights

### Enterprise AWS

- S3
- Bedrock Knowledge Bases
- OpenSearch Serverless/Aurora/MongoDB vector backend
- Bedrock model/agent
- Lambda/ECS/App Runner/API Gateway
- IAM/KMS/CloudWatch

### Private Local Server

- Docker Compose
- Qdrant or Postgres+pgvector
- Ollama for prototype or vLLM for throughput
- FastAPI
- reverse proxy with TLS
- local backups
