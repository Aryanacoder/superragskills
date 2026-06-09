# Deployment Playbooks

Use this when turning a RAG design into a deployable system.

## Local / Offline Server

Best for privacy, air-gap, field laptops, edge servers, demos, and low recurring cost.

Recommended architecture:

- FastAPI or equivalent backend
- Qdrant/Postgres+pgvector/FAISS/LanceDB
- object directory or MinIO for source files and page images
- Ollama for simple local serving, vLLM/TGI/NVIDIA stack for production GPU throughput
- Docker Compose for single-node; Kubernetes for multi-node/GPU pools
- offline model and index installation path

Checklist:

- pin model names and versions
- set max context, max upload size, and concurrency
- encrypt disk or object store if sensitive
- expose health endpoints for app, model server, vector DB
- schedule backups for metadata, vector DB, object files, and manifests
- define index replacement or migration procedure
- run eval suite after every knowledge pack update

## AWS

Best for AWS-native teams, S3 document lakes, IAM/KMS controls, Bedrock models, and managed RAG.

Managed path:

- S3 for source documents
- Bedrock Knowledge Bases for ingestion/chunking/embeddings/retrieval
- OpenSearch Serverless, Aurora PostgreSQL, MongoDB Atlas, or another supported vector backend
- Bedrock RetrieveAndGenerate or app-managed retrieval + generation
- API Gateway + Lambda, ECS/Fargate, App Runner, or EKS for app surface
- IAM least privilege, KMS encryption, CloudWatch logs/metrics

Custom path:

- S3 source bucket and asset bucket
- ECS/EKS ingestion workers
- OpenSearch/Qdrant/Milvus/Postgres vector store
- Bedrock or hosted/self-hosted model
- DynamoDB/Postgres metadata
- CloudWatch/X-Ray tracing

AWS checklist:

- region supports required Bedrock models and knowledge base features
- private networking/VPC endpoints if needed
- S3 bucket policies and object lifecycle configured
- KMS keys for storage and logs
- IAM roles separated for ingestion, query, admin
- index build is reproducible from source manifest
- blue/green index deployment or rollback plan exists

## Azure

Best for Microsoft enterprise environments, Entra ID, Azure OpenAI, Azure AI Search, Blob Storage, and App Service/Container Apps.

Classic managed path:

- Azure Blob Storage for documents
- Azure AI Search index with vector fields, full text fields, filters, semantic ranker
- Azure OpenAI embeddings and chat model
- App Service, Container Apps, AKS, or Functions for the app
- Managed Identity for service-to-service auth
- Key Vault for secrets
- Application Insights for traces

Agentic retrieval path:

- Azure AI Search knowledge base / agentic retrieval for query planning and parallel subqueries
- Azure OpenAI model for planning and generation
- app orchestrates chat state and final response

Azure checklist:

- choose classic RAG when simple single-index retrieval is enough
- choose agentic retrieval for complex multi-query workflows, noting preview/SLA status where applicable
- enable semantic ranker if relevant
- use managed identity instead of static keys where possible
- define index schema with filterable fields before ingesting
- configure private endpoints and network rules for enterprise deployments
- log query, retrieved docs, citations, token usage, and latency

## Kubernetes / Enterprise Self-Hosted

Use when:

- multiple services need independent scaling
- GPUs need scheduling
- zero-downtime updates matter
- enterprise monitoring and secrets are required

Services:

- web/API
- ingestion worker
- model gateway
- embedding service
- reranker service
- vector DB
- metadata DB
- object storage
- queue
- observability stack

Checklist:

- Helm/Kustomize manifests
- readiness/liveness probes
- resource requests/limits
- GPU node pools and model cache strategy
- persistent volumes and backups
- migration jobs
- canary or blue/green index switch
- Prometheus/Grafana/OpenTelemetry

## CI/CD And Release Gates

Every deployment path should include:

- lint/test/build
- schema migration check
- ingestion smoke test
- retrieval eval on golden set
- no-answer/refusal eval
- security scan
- environment variable validation
- rollback instructions
