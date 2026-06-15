# Notes

## RAG Pipeline Diagram

```mermaid
flowchart TD
    subgraph INGESTION["Ingestion Pipeline (Offline)"]
        A1["Organisational Data\n(PDFs, Docs, DBs, Web, APIs)"]
        A2["Parsing & Cleaning\n(Extract raw text, remove noise)"]
        A3["Chunking\n(Fixed-size / Semantic / Recursive)"]
        A4["Embedding Model\n(e.g. text-embedding-3-small)"]
        A5[("Vector DB\n(Pinecone / Weaviate / pgvector)")]

        A1 --> A2 --> A3 --> A4 --> A5
    end

    subgraph QUERY["Query Pipeline (Online)"]
        B1["User Query"]
        B2["Embed Query\n(same Embedding Model)"]
        B3["Similarity Search\n(Top-K retrieval)"]
        B4["Context Assembly\n(Retrieved chunks + metadata)"]
        B5["Prompt Construction\n(System prompt + context + query)"]
        B6["LLM\n(e.g. Claude / GPT-4)"]
        B7["Response to User"]

        B1 --> B2 --> B3 --> B4 --> B5 --> B6 --> B7
    end

    A5 -->|"Top-K chunks"| B3
    A4 -.->|"Shared model"| B2
```

Steps:
Vector Store (DB) pipeline: Data (Unstructured) -> Parsing -> Embedding -> Store

Parsing: Step which defines how to read the unstructured data and chunk it into smaller pieces. This is important because the embedding model has a maximum token limit, and we need to ensure that the chunks of text we create do not exceed this limit. The parsing step also helps to preserve the context of the original data, which can be important for downstream tasks such as search and retrieval

Embedding: Step which defines how to convert the parsed chunks of text into a numerical representation (vector) that can be stored in a vector database. This step typically involves using a pre-trained language model to generate embeddings for each chunk of text. The resulting vectors can then be stored in a vector database, which allows for efficient search and retrieval based on the similarity of the vectors.


