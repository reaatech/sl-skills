---
name: reaatech-hybrid-rag-qdrant
description: "These packages give you a complete, modular RAG stack built around Qdrant, with hybrid retrieval that combines vector search, BM25 keyword search, and cross-encoder reranking. You'd adopt them to avoid assembling and integrating a dozen…"
license: MIT
---

# REAA hybrid-rag-qdrant

These packages give you a complete, modular RAG stack built around Qdrant, with hybrid retrieval that combines vector search, BM25 keyword search, and cross-encoder reranking. You'd adopt them to avoid assembling and integrating a dozen separate tools for document ingestion, chunking, retrieval, reranking, evaluation, and observability into a coherent pipeline. The most distinctive thing is that every component—from four chunking strategies to ablation studies to an MCP server with 41 tools—is a separate, independently installable package sharing core types and Zod schemas, so you can use only what you need while keeping everything type-safe and composable.

## When to use this

Reach for this family when the task requires a production-grade hybrid RAG stack that combines vector search (Qdrant), BM25 keyword search, cross-encoder reranking, and configurable chunking strategies — all from a single set of packages. The family covers the full lifecycle: document ingestion and chunking, embedding generation across providers, hybrid retrieval, evaluation with standard IR metrics, ablation studies, benchmarking, observability (OpenTelemetry), and an MCP server exposing 40+ tools.

Concrete trigger phrases that map to this family:
- “hybrid RAG with Qdrant” / “vector + keyword search”
- “build a RAG pipeline with chunking, reranking, and evaluation”
- “set up Qdrant collections with BM25 and dense vectors”
- “run ablation studies or benchmarks on retrieval strategies”
- “expose RAG operations as MCP tools”

If the user mentions combining semantic search and keyword search, implementing cross-encoder reranking, or structuring a RAG evaluation workflow with metrics like NDCG, MAP, or MRR, this is the right choice. The CLI (`@reaatech/hybrid-rag-cli`) further reduces boilerplate for ingestion, querying, and benchmarking.

## Quick start example

The core flow is orchestrated by `RAGPipeline` from `@reaatech/hybrid-rag-pipeline`. Use it with an embedding service and a Qdrant collection to index documents and perform hybrid retrieval with optional reranking.

```typescript
import { RAGPipeline } from '@reaatech/hybrid-rag-pipeline';
import { EmbeddingService } from '@reaatech/hybrid-rag-embedding';

const embeddingService = new EmbeddingService({ provider: 'openai', apiKey: process.env.OPENAI_API_KEY });
const pipeline = new RAGPipeline({
  qdrantUrl: 'http://localhost:6333',
  embeddingService,
  collectionName: 'docs',
  rerankerModel: 'cross-encoder/ms-marco-MiniLM-L-6-v2',
});

await pipeline.ingest('./path/to/document.pdf', { chunkingStrategy: 'recursive' });
const results = await pipeline.query('What is hybrid RAG?', { topK: 5, rerank: true });
console.log(results.map(r => r.text));
```

## Don't reach for this when

- **You only need simple vector search without BM25 or reranking.** Use the lower-level `@reaatech/hybrid-rag-qdrant` wrapper or `@qdrant/js-client-rest` directly to avoid the overhead of a full RAG pipeline.
- **You’re building a chatbot or agent that requires LLM memory, tool calling, or conversation management.** This family focuses on retrieval infrastructure. Combine it with a framework like LangChain or the REAA `@reaatech/agent-core` family (if available) for conversational logic.
- **You only need an embedding service without any RAG orchestration.** Pull in just `@reaatech/hybrid-rag-embedding` — no need for the pipeline, Qdrant, or evaluation packages.
- **You need to perform evaluation or benchmarks on an existing RAG system that doesn’t use Qdrant.** Use `@reaatech/hybrid-rag-evaluation` in isolation (it accepts generic retrieval results

## Packages

```bash
npm install @reaatech/hybrid-rag @reaatech/hybrid-rag-cli @reaatech/hybrid-rag-embedding @reaatech/hybrid-rag-evaluation @reaatech/hybrid-rag-ingestion @reaatech/hybrid-rag-mcp-server @reaatech/hybrid-rag-observability @reaatech/hybrid-rag-pipeline @reaatech/hybrid-rag-qdrant @reaatech/hybrid-rag-retrieval
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/hybrid-rag` | published v0.1.0 | Zod schemas, TypeScript types, and enums for documents, chunks, retrieval results, evaluation samples, ablation configs, and benchmarking metrics that serve as the shared type f… |
| `@reaatech/hybrid-rag-cli` | published v0.1.1 | A CLI tool for hybrid RAG (Retrieval-Augmented Generation) workflows, providing commands for document ingestion, querying, evaluation, ablation studies, benchmarking, chunking p… |
| `@reaatech/hybrid-rag-embedding` | published v0.1.0 | A class that generates text embeddings through a provider-agnostic interface, currently supporting OpenAI models with built-in batch processing, rate limiting, and cost tracking. |
| `@reaatech/hybrid-rag-evaluation` | published v0.1.1 | An evaluation runner for hybrid RAG systems that provides standard IR metrics (Precision@K, Recall@K, NDCG@K, MAP, MRR), generation quality scoring, ablation studies with YAML-c… |
| `@reaatech/hybrid-rag-ingestion` | published v0.1.1 | A set of classes (`DocumentLoader`, `TextPreprocessor`, `DocumentValidator`) and a `chunkDocument` function for loading, preprocessing, validating, and chunking documents from P… |
| `@reaatech/hybrid-rag-mcp-server` | published v0.1.1 | An MCP server that exposes 41+ tools for hybrid RAG (vector + BM25) operations, including retrieval, ingestion, evaluation, query analysis, session management, and agent integra… |
| `@reaatech/hybrid-rag-observability` | published v0.1.1 | A Pino-based structured logger and OpenTelemetry tracing/metrics collector for hybrid RAG pipelines, providing pre-built helpers for logging query lifecycles, recording span dur… |
| `@reaatech/hybrid-rag-pipeline` | published v0.1.1 | A single `RAGPipeline` class that orchestrates document ingestion and hybrid retrieval (vector + BM25) against a Qdrant vector store, with optional reranking via Cohere, Jina, O… |
| `@reaatech/hybrid-rag-qdrant` | published v0.1.0 | A Qdrant vector database adapter that wraps `@qdrant/js-client-rest` with collection management, batch upsert, vector search with automatic metadata filtering, and health checks… |
| `@reaatech/hybrid-rag-retrieval` | published v0.1.1 | A hybrid retrieval engine that combines Qdrant vector search, in-process BM25 keyword search, cross-encoder reranking, and configurable score fusion (RRF, weighted sum, normaliz… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-hybrid-rag-qdrant`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/hybrid-rag-qdrant
- Browse packages: https://reaatech.com/products/domain-pipelines/hybrid-rag-qdrant/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, answer-relevance, benchmarks, context-precision, context-recall, faithfulness, information-retrieval, llm, llm-eval, rag, rag-evaluation, semantic-search, typescript, qdrant
