# AI Engineering Handbook

# Chapter: MCP, RAG, APIs, SDKs & Workflow Engines

## Learning Objectives

By the end of this chapter you should be able to:

-   Explain what MCP is and why it was created.
-   Understand Hosts, MCP Clients, MCP Servers, and Tools.
-   Explain the relationship between MCP, RAG, APIs, REST, SDKs, and
    workflow engines.
-   Explain how to expose a RAG pipeline as an MCP tool.
-   Answer common MCP interview questions.

------------------------------------------------------------------------

# 1. What is MCP?

**Model Context Protocol (MCP)** is an open protocol that standardizes
how AI applications communicate with external tools and data sources.

It does **not** replace APIs or REST. Instead, it provides a common
interface so AI applications can use tools consistently.

    Host (Claude, Cursor)
            ↓
       MCP Client
            ↓
       MCP Server
            ↓
     GitHub / Slack / RAG / Database

------------------------------------------------------------------------

# 2. MCP Components

## Host

The AI application the user interacts with.

Examples: - Claude Desktop - Cursor - VS Code AI extensions

The Host contains an MCP Client.

## MCP Client

Responsible for: - Discovering tools - Calling tools - Receiving results

The client **consumes** tools.

## MCP Server

Exposes tools to MCP clients.

Examples: - search_documents() - create_issue() - send_slack_message()

The server **provides** tools.

------------------------------------------------------------------------

# 3. Tool Discovery

The server registers tools using decorators like:

``` python
@mcp.tool()
def search_documents(query: str):
    ...
```

When the server starts, FastMCP registers the tool.

When a client connects it asks:

> "What tools do you provide?"

The server returns tool names, descriptions, and input schemas.

The LLM then decides whether to call a tool.

------------------------------------------------------------------------

# 4. RAG vs MCP

## RAG

Purpose: Retrieve relevant knowledge.

Pipeline:

    Documents
       ↓
    Chunking
       ↓
    Embeddings
       ↓
    Vector Database
       ↓
    Retrieved Chunks
       ↓
    LLM

## MCP

Purpose: Provide standardized access to tools.

Pipeline:

    LLM
     ↓
    MCP Client
     ↓
    MCP Server
     ↓
    Tool/API/Database

## Can MCP Replace RAG?

No.

-   RAG retrieves knowledge.
-   MCP standardizes communication.

A RAG system can be exposed through an MCP Server.

------------------------------------------------------------------------

# 5. Exposing RAG as an MCP Tool

Existing function:

``` python
def retrieve_documents(query):
    ...
```

Wrapper:

``` python
@mcp.tool()
def search_documents(query):
    return retrieve_documents(query)
```

Your retrieval logic stays the same.

The wrapper makes it discoverable by MCP clients.

------------------------------------------------------------------------

# 6. APIs, REST and SDKs

## API

A general interface that allows software to communicate.

Examples: - Python functions - REST APIs - GraphQL APIs - OS APIs

## REST

A style of web API that uses: - HTTP - URLs - GET - POST - PUT - PATCH -
DELETE

Example:

    GET /employees
    POST /tickets

## SDK

A Software Development Kit is a language-specific library that wraps
APIs.

Example:

Without SDK:

``` python
requests.post(...)
```

With SDK:

``` python
client.chat.completions.create(...)
```

Examples: - OpenAI Python SDK - boto3 - PyGithub

------------------------------------------------------------------------

# 7. FastMCP vs FastAPI

## FastAPI

Builds REST APIs.

## FastMCP

Builds MCP Servers.

It automatically: - registers tools - validates inputs - handles MCP
messages - serializes responses - starts the server

------------------------------------------------------------------------

# 8. Starting an MCP Server

``` python
mcp = FastMCP("Company RAG")

@mcp.tool()
def search_documents(query):
    ...

mcp.run()
```

Run:

``` bash
python mcp_server.py
```

------------------------------------------------------------------------

# 9. MCP Transports

## stdio

Local communication through stdin/stdout.

    Claude
       ↓
    Local MCP Server

## Streamable HTTP

Remote communication over HTTP.

    Claude
       ↓
    Internet
       ↓
    Remote MCP Server

------------------------------------------------------------------------

# 10. Workflow Engines

Workflow engines execute predefined steps.

Example:

    Upload PDF
       ↓
    Chunk
       ↓
    Embeddings
       ↓
    Vector DB

Examples: - Airflow - Temporal - Prefect - GitHub Actions - Jenkins

Workflow = fixed execution.

Agent = dynamic reasoning.

------------------------------------------------------------------------

# 11. Authentication vs Authorization

Authentication: \> Who are you?

Authorization: \> What are you allowed to access?

Example:

If Hema has the CEO role:

✅ CEO salary

If Hema is an engineer:

❌ CEO salary

Permissions are based on roles, groups, and policies.

------------------------------------------------------------------------

# 12. Production Security

Apply inside the MCP Server:

-   Authentication
-   Authorization
-   Metadata filtering
-   Tenant filtering
-   top_k limits
-   Output size limits
-   Audit logging
-   Secret redaction

------------------------------------------------------------------------

# 13. Latency, Token Cost and Context Window

## Latency

Time taken to produce an answer.

## Token Cost

The amount of text processed by the model.

Higher tokens: - higher cost - slower responses

## Context Window

Maximum amount of information the model can process at once.

Everything must fit: - system prompt - user prompt - retrieved chunks -
conversation history - tool outputs

------------------------------------------------------------------------

# 14. Interview Takeaways

-   MCP standardizes AI-to-tool communication.
-   MCP does not replace APIs.
-   MCP does not replace REST.
-   MCP does not replace RAG.
-   MCP Server exposes tools.
-   MCP Client invokes tools.
-   SDKs usually wrap REST APIs.
-   RAG retrieves knowledge.
-   MCP exposes capabilities.
