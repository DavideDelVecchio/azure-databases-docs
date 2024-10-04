---

title: Ingest and vectorize document files in Azure Cosmos DB
description: Learn how to ingest document files into Azure Cosmos DB for NoSQL
author: jcodella
ms.service: azure-cosmos-db
ms.subservice: nosql
ms.topic: quickstart
ms.date: 9/16/2024
ms.author: jacodel

---

# Load document files into Azure Cosmos DB

[!INCLUDE[NoSQL](../includes/appliesto-nosql.md)]

We introduce Doc2CDB for Azure Cosmos DB, a powerful accelerator designed to streamline the extraction, preprocessing, and management of large volumes of text data for vector similarity search. This solution uses the advanced vector indexing capabilities of Azure Cosmos DB and is powered by Azure AI Services to provide a robust and efficient pipeline that’s easily to set up and perfect for many use cases including:

- Vector Similarity Search over Text Data. Extract and vectorize text from document data to store in Azure Cosmos DB, makes it easy for you to perform semantic search to find documents that are contextually related to your queries. This allows them to discover relevant information that might not be found through traditional keyword searches, facilitating more comprehensive data retrieval.

- Retrieval-Augmented Generation (RAG) over Documents. Personalize your Small and Large Language Models to your data with RAG. By extracting text from document files, chunking and vectorizing the data, then storing it in Azure Cosmos DB, you’re then set up to empower the chatbot to generate more accurate and contextually relevant responses to your scenarios. When you ask a question, the chatbot retrieves the most relevant text chunks through vector search and uses them to generate an answer, grounded in your document data.


:::image type="content" source="../media/gen-ai/document-ingestion/document-ingestion-pipeline.png" alt-text="Diagram of the Cosmos AI Graph infrastructure, components, and flow.":::


## The end-to-end pipeline

Doc2CDB includes several key stages in its pipeline:
1. File Upload to Azure Blob Storage
   - The process begins with uploading documents to Azure Blob Storage. This stage ensures that your files are securely stored and easily accessible for further processing. This is compatible with PDFs, Microsoft Office documents (DOCX, XLSX, PPTX, HTML), and Images (JPEG, PNG, BMP, TIFF, HEIF). 
2. Text Extraction
   - Once the files are uploaded, the next step is text extraction. This involves parsing text data and performing OCR on documents using Azure Document Intelligence, to extract text that can be processed and indexed in Azure Cosmos DB. This stage is crucial for preparing the data for subsequent processing.
3. Text Chunking
   - After extraction, the raw text is broken down into manageable chunks. This chunking process is essential for enabling Small and Large Language Models (SLMs/LLMs) in Azure AI to process the text efficiently. By dividing the text into smaller pieces, we ensure that the data is more accessible and easier to handle.
4. Text Embedding
   - In this stage, Azure OpenAI Service’s text-3-embedding-large model is used to produce vector embeddings of the text chunks. These embeddings capture the semantic meaning of the text, allowing for more sophisticated and accurate searches. The embeddings are a critical component for enabling advanced search capabilities.
5. Text Storage
   - Finally, each text chunk, along with its corresponding vector embedding, is stored in an Azure Cosmos DB for NoSQL container as a unique document. This container is configured to perform efficient vector searches and, eventually, full-text searches. By using Azure Cosmos DB’s powerful vector indexing and search capabilities, users can quickly and easily retrieve relevant information from their text data.

## Benefits of the Doc2CDB solution accelerator
- Scalability: Handle large volumes of text data with ease, thanks to the scalable nature of Azure AI services and Azure Cosmos DB
- Efficiency: Streamline the text processing pipeline, reducing the time and effort required to manage and search text data. This is preconfigured for you
- Advanced Search Capabilities: Utilize ultra-fast and efficient Vector Indexing in Azure Cosmos DB perform vector search to find the most semantically relevant data from your documents

## Get started!

The Doc2CDB accelerator designed to help you parse, process, and store your document data more easily to take advantage of Azure Cosmos DB’s rich query language and powerful Vector Similarity Search.  Visit https://aka.ms/Doc2CDB and give it a try today!


## Next steps

Now that your documents are parsed, chunked, vectorized, and stored in Azure Cosmos DB for NoSQL, check out these resources here to get started with vector search and building AI apps:

- [Vector Search with Azure Cosmos DB for NoSQL](vector-search-overview.md)
- [Tokens](tokens.md)
- [Vector Embeddings](vector-embeddings.md)
- [Retrieval Augmented Generated (RAG)](rag.md)
- [30-day Free Trial without Azure subscription](https://azure.microsoft.com/try/cosmosdb/)
- [90-day Free Trial and up to $6,000 in throughput credits with Azure AI Advantage](../ai-advantage.md)

> [!div class="nextstepaction"]
> [Use the Azure Cosmos DB lifetime free tier](../free-tier.md)
