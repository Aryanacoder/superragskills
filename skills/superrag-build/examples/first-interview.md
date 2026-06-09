# Example: First Interview

User:

> I want a RAG app for my company knowledge.

Agent:

> I will use superRAG-build to choose the right RAG architecture before coding. First I need the shape of the system:
>
> 1. What should users be able to decide or do after asking the app?
> 2. Who will use it, and what would be costly if the answer is wrong?
> 3. What data do you have: PDFs, Office files, databases, tickets, websites, code, images, tables, or APIs?
> 4. How often does the data change?
> 5. Must answers include citations, source previews, exact quotes, downloads, or audit logs?
> 6. Does it need to run locally/offline, on AWS, on Azure, or on another server?
> 7. Do you already prefer a stack such as Bedrock, Azure AI Search, OpenAI file search, Qdrant, LangChain, or LlamaIndex?
> 8. What first demo question must work?
>
> Based on those answers, I will classify whether you need basic RAG, hybrid/reranked RAG, metadata-filtered RAG, GraphRAG, agentic RAG, multimodal RAG, structured-data RAG, or local/offline RAG.
