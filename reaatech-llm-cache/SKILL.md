---
name: reaatech-llm-cache
description: "These packages give you a semantic caching layer for LLM calls that returns cached responses for both exact prompt matches and semantically similar prompts above a configurable cosine similarity threshold. You'd adopt them to reduce API…"
license: MIT
---

# REAA llm-cache

These packages give you a semantic caching layer for LLM calls that returns cached responses for both exact prompt matches and semantically similar prompts above a configurable cosine similarity threshold. You'd adopt them to reduce API costs and latency by avoiding redundant LLM calls, especially when users ask the same question in different phrasings. The system is built as a modular engine with pluggable storage adapters (Redis, DynamoDB, Qdrant) and optional cost tracking, observability, and HTTP server packages that compose together through well-defined interfaces rather than a monolithic service.

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
| `@reaatech/llm-cache` | published v0.1.0 | A caching engine for LLM calls that provides both exact-match (SHA-256 hash) and semantic (cosine similarity on embeddings) cache lookups, with model-aware fingerprinting, use-c… |
| `@reaatech/llm-cache-adapters-dynamodb` | published v0.1.0 | A DynamoDB storage adapter for `@reaatech/llm-cache` that persists exact-match cache entries with native TTL, GSI-backed metadata queries, and batch operations chunked to AWS li… |
| `@reaatech/llm-cache-adapters-qdrant` | published v0.1.0 | A Qdrant vector database adapter for `@reaatech/llm-cache` that implements the `VectorStorageAdapter` interface, providing HNSW approximate nearest neighbor search with metadata… |
| `@reaatech/llm-cache-adapters-redis` | published v0.1.0 | A Redis storage adapter for the `@reaatech/llm-cache` library that implements the `StorageAdapter` interface, providing exact-match cache operations with automatic TTL via `SETE… |
| `@reaatech/llm-cache-cost-tracker` | published v0.1.0 | A cost calculator and pricing database for LLM API usage, providing a `CostCalculator` class that computes per-request costs from token counts and model pricing, and tracks savi… |
| `@reaatech/llm-cache-observability` | published v0.1.0 | A structured JSON logger and Prometheus-compatible metrics collector for LLM cache operations, providing automatic PII redaction on 17 sensitive field names, correlation ID prop… |
| `@reaatech/llm-cache-server` | published v0.1.0 | An HTTP server wrapper for llm-cache that exposes a REST API for cache operations, Prometheus metrics, and health endpoints, configurable via environment variables for storage (… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-llm-cache`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/llm-cache
- Browse packages: https://reaatech.com/products/observability-cost/llm-cache/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, embeddings, llm, llm-cache, openai, semantic-search, typescript
