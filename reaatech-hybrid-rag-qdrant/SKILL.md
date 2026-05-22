---
name: reaatech-hybrid-rag-qdrant
description: "These packages give you a complete hybrid RAG system combining vector search (Qdrant), BM25 keyword search, and cross-encoder reranking with four chunking strategies. They solve the problem of building a production RAG stack that include…"
license: MIT
---

# REAA hybrid-rag-qdrant

These packages give you a complete hybrid RAG system combining vector search (Qdrant), BM25 keyword search, and cross-encoder reranking with four chunking strategies. They solve the problem of building a production RAG stack that includes retrieval, evaluation, benchmarking, and agent integration without stitching together disparate tools.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Domain Pipelines** category. 10 packages live under `@reaatech/hybrid-rag` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/hybrid-rag @reaatech/hybrid-rag-cli @reaatech/hybrid-rag-embedding @reaatech/hybrid-rag-evaluation @reaatech/hybrid-rag-ingestion @reaatech/hybrid-rag-mcp-server @reaatech/hybrid-rag-observability @reaatech/hybrid-rag-pipeline @reaatech/hybrid-rag-qdrant @reaatech/hybrid-rag-retrieval
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/hybrid-rag` | published v0.1.0 | Shared TypeScript types and Zod schemas for documents, chunks, retrieval, evaluation, and benchmarking in a hybrid RAG system; requires only `zod` at runtime. |
| `@reaatech/hybrid-rag-cli` | published v0.1.0 | Command-line interface for hybrid RAG systems that provides commands for document ingestion, querying, evaluation, ablation studies, benchmarking, and MCP server startup, with a… |
| `@reaatech/hybrid-rag-embedding` | published v0.1.0 | Provides a unified `EmbeddingService` class that generates embeddings for hybrid RAG systems across multiple providers (OpenAI, with extension points for Vertex AI and local mod… |
| `@reaatech/hybrid-rag-evaluation` | published v0.1.0 | Evaluates and benchmarks hybrid RAG systems using standard IR metrics (Precision@K, NDCG, MAP, MRR), generation quality scores, and configurable ablation studies with delta comp… |
| `@reaatech/hybrid-rag-ingestion` | published v0.1.0 | Provides a suite of classes and functions for loading, normalizing, validating, and chunking documents into formats suitable for RAG pipelines. It supports four distinct chunkin… |
| `@reaatech/hybrid-rag-mcp-server` | published v0.1.0 | Exposes over 40 Model Context Protocol (MCP) tools for managing RAG lifecycles, including retrieval, ingestion, evaluation, and observability. It provides a `createMCPServer` fu… |
| `@reaatech/hybrid-rag-observability` | published v0.1.0 | Provides structured logging, OpenTelemetry tracing, and metrics collection specifically for hybrid RAG pipelines. It exports a set of utility functions and managers that wrap Pi… |
| `@reaatech/hybrid-rag-pipeline` | published v0.1.0 | A single `RAGPipeline` class that orchestrates document ingestion and hybrid (vector + BM25) retrieval with optional reranking, backed by Qdrant and configurable embedding provi… |
| `@reaatech/hybrid-rag-qdrant` | published v0.1.0 | Provides a wrapper class for the Qdrant REST client that simplifies collection management, batch upserting, and metadata-filtered vector searches. It acts as an abstraction laye… |
| `@reaatech/hybrid-rag-retrieval` | published v0.1.0 | Orchestrates hybrid RAG pipelines by combining Qdrant-based vector search, in-process BM25 keyword search, and cross-encoder reranking. It provides a `HybridRetriever` class tha… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-hybrid-rag-qdrant`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/hybrid-rag-qdrant
- Browse packages: https://reaatech.com/products/domain-pipelines/hybrid-rag-qdrant/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, answer-relevance, benchmarks, context-precision, context-recall, faithfulness, information-retrieval, llm, llm-eval, rag, rag-evaluation, semantic-search, typescript, qdrant
