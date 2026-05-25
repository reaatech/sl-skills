---
name: reaatech-idempotency-middleware
description: "These packages give you idempotency middleware for POST, PUT, and PATCH requests — duplicate requests with the same `Idempotency-Key` header return the cached original response instead of re-executing side effects. You'd adopt them to sa…"
license: MIT
---

# REAA idempotency-middleware

These packages give you idempotency middleware for POST, PUT, and PATCH requests — duplicate requests with the same `Idempotency-Key` header return the cached original response instead of re-executing side effects. You'd adopt them to safely retry payment charges, webhook deliveries, or any mutation where duplicate execution would cause data corruption or duplicate charges. The core provides a pluggable `StorageAdapter` interface with in-memory, Redis, DynamoDB, and Firestore backends, plus framework adapters for Express, Koa, and a generic handler for Lambda or gRPC, all with built-in distributed locking that prevents concurrent handler execution for the same key.

## When to use this

Reach for the `@reaatech/idempotency-middleware` family when an HTTP API must safely handle duplicate requests for mutating endpoints (`POST`, `PUT`, `PATCH`). The core use case is preventing double side effects—duplicate charges, repeated resource creation, state corruption—when clients cannot guarantee exactly-once delivery. The library solves this by caching the first response keyed to the `Idempotency-Key` header and replaying it for all subsequent requests with the same key, without re-executing the handler.

Trigger phrases that map to this family include:

- "Make POST/PUT/PATCH requests idempotent"
- "Retry safety with Idempotency-Key header"
- "Prevent duplicate side effects on network retries"
- "Cache and replay responses for a given idempotency key"

This family is also the right choice when you need distributed locking across replicas to guarantee that only one request with a given idempotency key is processed at a time. The built-in lock mechanism prevents race conditions during concurrent retries. The pluggable storage adapters (in-memory, Redis, DynamoDB, Firestore) allow you to match the persistence and scalability needs of your deployment—ephemeral for testing, Redis for latency-sensitive production, DynamoDB or Firestore for serverless or cloud-native stacks.

## Quick start example

The most common setup is adding idempotency to an Express app using an in-memory store (for development or single-instance deployments). Import the core adapter and the Express middleware factory:

```typescript
import { InMemoryAdapter } from '@reaatech/idempotency-middleware';
import { createExpressMiddleware } from '@reaatech/idempotency-middleware-express';

const app = require('express')();

const adapter = new InMemoryAdapter();

app.post('/payments', createExpressMiddleware({ adapter }), async (req, res) => {
  // This handler will only execute once per Idempotency-Key
  const result = await processPayment(req.body);
  res.json(result);
});

app.listen(3000);
```

For production, swap the `InMemoryAdapter` with a Redis, DynamoDB, or Firestore adapter. The middleware configuration remains the same.

## Don't reach for this when

- **You need idempotency across multiple independent services (e.g., a saga).** This library scopes idempotency to a single HTTP request/reply pair per service. For multi-step distributed transactions, use a saga pattern or the `@reaatech/outbox` family to coordinate state across services.

- **Your use case is inherently idempotent at the database level** (e.g., `INSERT … ON CONFLICT DO NOTHING` or `SET x = x`). Adding an idempotency middleware adds unnecessary latency and storage overhead; rely on database constraints instead.

- **You

## Packages

```bash
npm install @reaatech/idempotency-middleware @reaatech/idempotency-middleware-adapter-dynamodb @reaatech/idempotency-middleware-adapter-firestore @reaatech/idempotency-middleware-adapter-redis @reaatech/idempotency-middleware-express @reaatech/idempotency-middleware-koa
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/idempotency-middleware` | published v1.0.0 | A framework-agnostic TypeScript middleware that makes POST, PUT, and PATCH requests idempotent by caching responses keyed to an `Idempotency-Key` header, with pluggable storage… |
| `@reaatech/idempotency-middleware-adapter-dynamodb` | published v1.0.0 | A DynamoDB storage adapter for `@reaatech/idempotency-middleware` that implements the `StorageAdapter` interface using conditional writes for distributed locking and TTL-compati… |
| `@reaatech/idempotency-middleware-adapter-firestore` | published v1.0.0 | A Firestore storage adapter for `@reaatech/idempotency-middleware` that provides transaction-gated distributed locking and TTL-compatible expiry via `expiresAt` Date fields. Exp… |
| `@reaatech/idempotency-middleware-adapter-redis` | published v1.0.0 | Redis storage adapter for `@reaatech/idempotency-middleware` that implements the `StorageAdapter` interface using ioredis, providing distributed idempotency caching with token-g… |
| `@reaatech/idempotency-middleware-express` | published v1.0.0 | Express middleware that caches responses keyed by the `Idempotency-Key` header, preventing duplicate processing of retried requests. It monkey-patches `res.json()` and `res.send… |
| `@reaatech/idempotency-middleware-koa` | published v1.0.0 | Koa middleware that caches responses keyed by the `Idempotency-Key` header, preventing duplicate processing of the same request. It provides an `idempotentKoa(storage, config?)`… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-idempotency-middleware`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/idempotency-middleware
- Browse packages: https://reaatech.com/products/reliability-ops/idempotency-middleware/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: aws, developer-tools, docker, typescript
