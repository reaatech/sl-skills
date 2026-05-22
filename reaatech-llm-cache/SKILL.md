---
name: reaatech-llm-cache
description: "These packages provide a semantic and exact-match caching layer for LLM interactions, including support for embedding-based similarity, model-aware fingerprinting, and cost tracking. You would adopt them to reduce latency and API expense…"
license: MIT
---

# REAA llm-cache

These packages provide a semantic and exact-match caching layer for LLM interactions, including support for embedding-based similarity, model-aware fingerprinting, and cost tracking. You would adopt them to reduce latency and API expenses by serving cached responses for semantically equivalent prompts across different storage backends like Redis, DynamoDB, and Qdrant. The system is built around a modular architecture where a core engine composes pluggable storage adapters, cost calculators, and observability utilities to fit into either application code or as a standalone HTTP sidecar.

## When to use this

Reach for the **llm-cache** family when your agent generates or consumes LLM responses and you want to eliminate redundant API calls to reduce latency and cost. The core use case is serving cached responses for semantically equivalent prompts — for example, two users asking “What’s the weather in Tokyo?” and “weather tokyo now” should hit the same cached response. The cache engine performs exact-match lookups first, then falls back to embedding‑based similarity search using a pluggable vector adapter. It also supports model‑aware fingerprinting so that the same prompt to different model versions produces separate cache entries.

**Trigger phrases that map to this family:**
- “Cache LLM responses to avoid repeated API calls”
- “Reduce OpenAI / Anthropic costs with semantic caching”
- “Store and retrieve LLM outputs by embedding similarity”
- “Track cost savings from cache hits across models”
- “Deploy a cached LLM sidecar with Redis / DynamoDB / Qdrant”

The family is modular: you pick a storage backend (`RedisAdapter`, `DynamoDBAdapter`, `QdrantAdapter`), optionally add a `CostCalculator`, and attach observability with `Logger` and `MetricsCollector`. The `llm-cache-server` package exposes the same engine as a REST API, suitable for sidecar or microservice deployments.

## Quick start example

```typescript
import { CacheEngine } from '@reaatech/llm-cache';
import { RedisAdapter } from '@reaatech/llm-cache-adapters-redis';
import { CostCalculator } from '@reaatech/llm-cache-cost-tracker';

const storage = new RedisAdapter({ url: 'redis://localhost:6379' });
const costCalc = new CostCalculator();
const engine = new CacheEngine({ storage, embedder: /* your embedder function */, costCalculator: costCalc });

// Lookup a response; returns cached result if semantically similar exists
const result = await engine.lookup('What is the capital of France?');
if (result.hit) {
  console.log('Cache hit, saved $', result.costSaved);
} else {
  // Call LLM, then store response
  await engine.store('What is the capital of France?', 'Paris', { model: 'gpt-4o' });
}
```

## Don’t reach for this when

- **You need a generic key‑value cache for non‑LLM data.** Use Redis, DynamoDB, or another general‑purpose cache directly. This family is tightly coupled to LLM response patterns (embedding comparison, model fingerprint, response metadata).
- **Your LLM calls are streaming or require real‑time buffering.** The cache stores full responses; streaming responses must be fully consumed before caching. If you need token‑by‑token passthrough without storing, use a proxy or gateway library instead.
- **You require fine‑grained per‑user cost allocation.** The `CostCalculator` estimates savings per cache hit but does not track per‑request user attribution. For billing‑grade cost breakdowns, wire your own metering layer on top.
- **You have no need for semantic similarity matching.** If exact‑match only (e.g., literal prompt comparison) is enough, a simple key‑value store with a hash of the prompt is far simpler and avoids embedding overhead.
- **You are not using OpenAI, Anthropic, or other LLM providers.** The cost calculator and model fingerprinting are provider‑specific. For custom models or non‑LLM embeddings, you would need to implement your own `Embedder` and cost logic from scratch.

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
