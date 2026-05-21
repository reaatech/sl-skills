---
name: reaatech-idempotency-middleware
description: "These packages add idempotency to HTTP APIs by caching responses keyed to the `Idempotency-Key` header, so retrying a `POST`, `PUT`, or `PATCH` request returns the original result instead of re-executing side effects. You’d adopt them to…"
license: MIT
---

# REAA idempotency-middleware

These packages add idempotency to HTTP APIs by caching responses keyed to the `Idempotency-Key` header, so retrying a `POST`, `PUT`, or `PATCH` request returns the original result instead of re-executing side effects. You’d adopt them to prevent duplicate charges, double-submissions, or inconsistent state when clients can’t guarantee exactly-once delivery. The collection is distinctive for its pluggable storage adapters (in-memory, Redis, DynamoDB, Firestore) behind a single interface, built-in distributed locking, and first-class middleware for Express, Koa, and generic handlers (Lambda, queues, gRPC) that all share the same core logic.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Reliability & Ops** category. 6 packages live under `@reaatech/idempotency-middleware` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/idempotency-middleware` | published v1.0.0 | Prevents duplicate execution of side-effecting operations by caching results and enforcing distributed locking based on an idempotency key. It provides a core `IdempotencyMiddle… |
| `@reaatech/idempotency-middleware-adapter-dynamodb` | published v1.0.0 | A DynamoDB-backed `StorageAdapter` for `@reaatech/idempotency-middleware` that uses conditional writes for distributed locking and stores TTL-compatible `expiresAt` attributes,… |
| `@reaatech/idempotency-middleware-adapter-firestore` | published v1.0.0 | Provides a Firestore storage adapter for `@reaatech/idempotency-middleware` that uses atomic transactions to manage distributed locks and cached responses. It exports a `Firesto… |
| `@reaatech/idempotency-middleware-adapter-redis` | published v1.0.0 | A Redis-backed storage adapter for `@reaatech/idempotency-middleware`, providing idempotency records and token‑guarded locks via an ioredis client. |
| `@reaatech/idempotency-middleware-express` | published v1.0.0 | Express middleware that caches responses keyed by the `Idempot |
| `@reaatech/idempotency-middleware-koa` | published v1.0.0 | Koa middleware that caches responses keyed by the `Idempotency-Key` header, ensuring safe retries for mutating routes by replaying the cached status, body, and headers without r… |

## Quick start

```bash
npm install @reaatech/idempotency-middleware @reaatech/idempotency-middleware-adapter-dynamodb @reaatech/idempotency-middleware-adapter-firestore @reaatech/idempotency-middleware-adapter-redis @reaatech/idempotency-middleware-express @reaatech/idempotency-middleware-koa
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-idempotency-middleware`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/idempotency-middleware
- Browse packages: https://reaatech.com/products/reliability-ops/idempotency-middleware/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: aws, developer-tools, docker, typescript
