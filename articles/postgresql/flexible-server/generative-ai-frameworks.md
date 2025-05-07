---
title: GenAI Frameworks and Azure Database for PostgreSQL
description: Integrate Azure Databases for PostgreSQL with AI and large language model (LLM) orchestration packages like Semantic Kernel and LangChain.
author: abeomor
ms.author: abeomorogbe
ms.date: 03/17/2025
ms.service: azure-database-postgresql
ms.subservice: flexible-server
ms.collection: ce-skilling-ai-copilot
ms.custom:
  - build-2025
ms.topic: conceptual
---

# Azure Database for PostgreSQL integrations for AI applications

Azure Database for PostgreSQL seamlessly integrates with leading large language model (LLM) orchestration packages like [Semantic Kernel](https://github.com/microsoft/semantic-kernel) and [LangChain](https://www.langchain.com/), enabling developers to harness the power of advanced AI capabilities within their applications. These orchestration packages can streamline the management and use of LLMs, embedding models, and databases, making it even easier to develop Generative AI applications.

| Integration Tool | Description | Azure Database for PostgreSQL | 
| --- | --- | --- |
| **[Semantic Kernel](https://github.com/microsoft/semantic-kernel)** | An open-source framework by Microsoft that combines AI agents with languages like C#, Python, and Java, enabling seamless orchestration of code and AI models. | [Python Connector](/semantic-kernel/concepts/vector-store-connectors/out-of-the-box-connectors/postgres-connector?pivots=programming-language-python) <br> [.NET Connector](/semantic-kernel/concepts/vector-store-connectors/out-of-the-box-connectors/postgres-connector?pivots=programming-language-csharp) <br> [Java Connector](/semantic-kernel/concepts/vector-store-connectors/out-of-the-box-connectors/postgres-connector?pivots=programming-language-java)
| **[LangChain](https://www.langchain.com/)** | A framework that simplifies the creation of applications powered by large language models (LLMs), offering tools for context-aware reasoning applications in Python, JavaScript, and Java. | [Python](generative-ai-develop-with-langchain.md) <br> [JavaScript](https://js.langchain.com/docs/integrations/vectorstores/pgvector/) | 
| **[LlamaIndex](https://www.llamaindex.ai/)** | A framework for building context-augmented AI applications that can integrate private or domain-specific data with LLMs for complex workflows. | [Python](https://docs.llamaindex.ai/en/stable/examples/vector_stores/postgres/) | 
| **[Microsoft GraphRAG](https://microsoft.github.io/graphrag/)** | Uses Azure Database for PostgreSQL to create AI-powered knowledge graphs, enabling robust data models and revealing relationships in semi-structured data. | [Quickstart](https://github.com/Azure-Samples/graphrag-legalcases-postgres/) | 

## Related content

- [Using LangChain with Azure Database for PostgreSQL](generative-ai-develop-with-langchain.md)
- [AI Agents in Azure Database for PostgreSQL](generative-ai-agents.md)
- [Learn more about Azure OpenAI Service integration](generative-ai-azure-openai.md)
- [Generative AI with Azure Database for PostgreSQL flexible server](generative-ai-overview.md).
- [Enable and use pgvector in Azure Database for PostgreSQL flexible server](how-to-use-pgvector.md).
