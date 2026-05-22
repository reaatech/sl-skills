---
name: reaatech-llm-cache
description: "These packages provide a semantic and exact-match caching layer for LLM interactions, including support for embedding-based similarity, model-aware fingerprinting, and cost tracking. You would adopt them to reduce latency and API expense…"
license: MIT
---

# REAA llm-cache

These packages provide a semantic and exact-match caching layer for LLM interactions, including support for embedding-based similarity, model-aware fingerprinting, and cost tracking. You would adopt them to reduce latency and API expenses by serving cached responses for semantically equivalent prompts across different storage backends like Redis, DynamoDB, and Qdrant. The system is built around a modular architecture where a core engine composes pluggable storage adapters, cost calculators, and observability utilities to fit into either application code or as a standalone HTTP sidecar.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Observability & Cost** category. 7 packages live under `@reaatech/llm-cache` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/llm-cache @reaatech/llm-cache-adapters-dynamodb @reaatech/llm-cache-adapters-qdrant @reaatech/llm-cache-adapters-redis @reaatech/llm-cache-cost-tracker @reaatech/llm-cache-observability @reaatech/llm-cache-server
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/llm-cache` | published v0.1.0 | Provides a multi-stage caching engine for LLM responses that performs exact-match lookups and embedding-based semantic similarity searches. It exposes a `CacheEngine` class that… |
| `@reaatech/llm-cache-adapters-dynamodb` | published v0.1.0 | Provides a DynamoDB storage adapter class for the `llm-cache` library, enabling persistent caching with native TTL support and GSI-based metadata querying. It implements the `St… |
| `@reaatech/llm-cache-adapters-qdrant` | published v0.1.0 | Provides a Qdrant vector database adapter for the `llm-cache` library, implementing the `VectorStorageAdapter` interface for semantic search and metadata filtering. It exposes a… |
| `@reaatech/llm-cache-adapters-redis` | published v0.1.0 | Provides a Redis storage adapter for the `llm-cache` library, enabling persistent key-value caching with support for batch operations and metadata-based invalidation. It exports… |
| `@reaatech/llm-cache-cost-tracker` | published v0.1.0 | Calculates LLM request costs and cache savings using a built-in database of pricing for over 40 models. It provides a `CostCalculator` class that implements the `CostCalculatorL… |
| `@reaatech/llm-cache-observability` | published v0.1.0 | Provides structured NDJSON logging with automatic PII redaction and Prometheus-compatible metrics collection for the `llm-cache` library. It exposes `Logger` and `MetricsCollect… |
| `@reaatech/llm-cache-server` | published v0.1.0 | Provides a REST API server for managing LLM cache operations, including semantic search and exact-match lookups. It exposes a configurable HTTP interface that supports Redis, Dy… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-llm-cache`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/llm-cache
- Browse packages: https://reaatech.com/products/observability-cost/llm-cache/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, embeddings, llm, llm-cache, openai, semantic-search, typescript
