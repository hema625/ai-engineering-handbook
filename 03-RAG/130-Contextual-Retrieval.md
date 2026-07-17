# Contextual Retrieval

**Volume:** 03-RAG\
**Chapter:** 08-Contextual-Retrieval

## Folder Location

``` text
03-RAG/
├── README.md
├── 01-Introduction.md
├── 02-Chunking.md
├── 03-Embeddings.md
├── 04-Vector-Databases.md
├── 05-BM25.md
├── 06-Hybrid-Search.md
├── 07-Reranking.md
├── 08-Contextual-Retrieval.md
├── 09-Late-Chunking.md
├── 10-Graph-RAG.md
├── 11-Agentic-RAG.md
├── diagrams/
├── examples/
└── interview/
```

# 1. Definition

Contextual Retrieval enriches every chunk with an LLM-generated
description before indexing. The stored content becomes:

**Context + Original Chunk**

This preserves document-level information lost during chunking.

# 2. Why was it introduced?

Traditional chunking removes: - document title - section heading -
company name - date

The answer may exist in a chunk but still not be retrieved.

# 3. ELI5

A newspaper clipping saying 'Revenue increased' is confusing by itself.
Add 'This is from ACME's Q2 report' before storing it, and the clipping
becomes understandable.

# 4. Architecture

Document → Chunk → LLM Generates Context → Context + Chunk → Embedding +
BM25 Index → Hybrid Search → Reranker → LLM

# 5. Workflow

1.  Upload document.
2.  Split into chunks.
3.  Generate context for every chunk.
4.  Store Context + Chunk.
5.  Build Vector Index.
6.  Build BM25 Inverted Index.
7.  Retrieve using Hybrid Search.
8.  Rerank.
9.  Generate answer.

# 6. ACME Example

Question: What drove ACME's Q2 revenue growth?

Answer: Revenue growth was driven mainly by cloud services.

The problem is retrieval because the original chunk doesn't mention ACME
or Q2.

# 7. Important Concepts

  Concept         Meaning
  --------------- ---------------------------
  Chunk           Small document section
  Context         LLM-generated description
  Vector DB       Semantic search
  BM25            Keyword search
  Hybrid Search   Vector + BM25
  Reranker        Better ranking

# 8. Advantages

-   Better retrieval
-   Better semantic understanding
-   Better keyword matching
-   Lower RAG failures

# 9. Limitations

-   One LLM call per chunk
-   Larger index
-   Higher indexing cost

# 10. Interview Questions

-   What is Contextual Retrieval?
-   Why is it useful?
-   Why is context generated once?
-   Why use BM25 and vectors together?

# 11. Related Topics

-   Chunking
-   BM25
-   Hybrid Search
-   Reranking
-   Agentic RAG

# 12. Key Takeaways

-   Context is generated for every chunk.
-   It is created once during indexing.
-   Contextualized chunks are stored in both Vector DB and BM25 indexes.
