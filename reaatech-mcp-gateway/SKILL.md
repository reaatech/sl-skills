---
name: reaatech-mcp-gateway
description: "These packages provide a modular gateway framework for managing Model Context Protocol (MCP) infrastructure, including authentication, rate limiting, schema validation, and fan-out routing. They allow you to secure, observe, and scale co…"
license: MIT
---

# REAA mcp-gateway

These packages provide a modular gateway framework for managing Model Context Protocol (MCP) infrastructure, including authentication, rate limiting, schema validation, and fan-out routing. They allow you to secure, observe, and scale connections to upstream MCP servers by composing individual Express-compatible middleware components. The collection is designed as a set of independently versioned packages that share a unified configuration schema and core type system, enabling you to build a custom gateway or deploy the pre-configured server.

## When to use this

Reach for the **mcp-gateway** package family when the task involves exposing, securing, or scaling a Model Context Protocol (MCP) server behind an Express-based gateway. The user explicitly mentions “MCP gateway”, “MCP authentication”, “MCP rate limiting”, “MCP tool access control”, “MCP audit logging”, “MCP request fan-out”, or “MCP schema validation”. The collection solves the problem of composing authentication, rate limiting, caching, auditing, validation, and upstream routing middleware into a single HTTP endpoint that proxies JSON-RPC 2.0 MCP calls to one or more backend MCP servers.

Use these packages when you need to enforce per-tenant allowlists for tool calls (`@reaatech/mcp-gateway-allowlist`), validate API keys or JWTs (`@reaatech/mcp-gateway-auth`), throttle requests with token buckets (`@reaatech/mcp-gateway-rate-limit`), cache responses per tool (`@reaatech/mcp-gateway-cache`), store tamper-evident audit logs (`@reaatech/mcp-gateway-audit`), or route calls across multiple upstream servers with strategies like first-success or majority-vote (`@reaatech/mcp-gateway-fanout`). The pre-configured `@reaatech/mcp-gateway-gateway` package ties everything together as a single server with a CLI, while the core package provides shared types and validation schemas.

Common trigger patterns in a prompt: “build an MCP gateway that requires authentication”, “add rate limiting to our existing MCP proxy”, “log every MCP request for compliance”, “distribute MCP requests across multiple model providers”, or “validate MCP tool arguments before forwarding”.

## Quick start example

The fastest path is to use the pre-configured gateway server from `@reaatech/mcp-gateway-gateway`. The following example creates an app with default auth, rate limiting, and observability, and listens on port 3000.

```typescript
import { createApp } from '@reaatech/mcp-gateway-gateway';
import { createLogger } from '@reaatech/mcp-gateway-core';

const logger = createLogger({ name: 'my-gateway' });

const app = await createApp({
  tenantRegistryPath: './tenants.yaml',
  auth: { apiKeys: true },
  rateLimit: { requestsPerMinute: 60 },
  observability: { enabled: true },
}, { logger });

app.listen(3000, () => {
  logger.info('MCP gateway listening on port 3000');
});
```

For a modular setup, import individual middleware packages (e.g., `@reaatech/mcp-gateway-auth`, `@reaatech/mcp-gateway-rate-limit`) and mount them on an Express app directly. Each middleware adheres to the standard `(req, res, next)` signature and expects MCP JSON-RPC 2.0 request bodies.

## Don't reach for this when

- You are building a non-MCP API gateway for REST or GraphQL. This family is tightly coupled to the MCP protocol (JSON-RPC 2.0 with tools/resources concepts). Use a general-purpose gateway like Express native middleware, `express-gateway`, or Envoy instead.
- You need a client-side MCP library to call tools from an MCP server. The `mcp-gateway` packages are server-side only; look for MCP client SDKs (e.g., the official `@modelcontextprotocol/sdk`).
- You require WebSocket

## Packages

```bash
npm install @reaatech/mcp-gateway-allowlist @reaatech/mcp-gateway-audit @reaatech/mcp-gateway-auth @reaatech/mcp-gateway-cache @reaatech/mcp-gateway-core @reaatech/mcp-gateway-fanout @reaatech/mcp-gateway-gateway @reaatech/mcp-gateway-observability @reaatech/mcp-gateway-rate-limit @reaatech/mcp-gateway-validation
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-gateway-allowlist` | published v1.0.0 | Enforces per-tenant tool access control for MCP gateways using wildcard pattern matching and versioned allowlists. It provides a utility function for access checks and an Expres… |
| `@reaatech/mcp-gateway-audit` | published v1.0.0 | Captures and stores security-relevant audit events for the MCP Gateway using a set of logger classes and a query service. It provides structured JSON output, tamper-evident SHA-… |
| `@reaatech/mcp-gateway-auth` | published v1.0.0 | Provides Express middleware that validates API keys, JWTs, OAuth2 introspection, or OIDC tokens to secure MCP Gateway requests. It attaches a typed `AuthContext` to the request… |
| `@reaatech/mcp-gateway-cache` | published v1.0.0 | Provides response caching for MCP gateways using either an in-memory LRU or Redis backend. It exports an Express middleware and a `CacheManager` class that supports per-tool TTL… |
| `@reaatech/mcp-gateway-core` | published v1.0.0 | Provides shared TypeScript interfaces, Zod validation schemas, and configuration management utilities for the MCP Gateway ecosystem. It includes SSRF-protected URL validation, Y… |
| `@reaatech/mcp-gateway-fanout` | published v1.0.0 | Orchestrate requests across multiple MCP server upstreams using fan-out strategies like first-success, all-wait, or majority-vote. This library provides functions for weighted u… |
| `@reaatech/mcp-gateway-gateway` | published v1.0.0 | Provides a full-featured Model Context Protocol (MCP) gateway server as an Express 5 application, complete with built-in authentication, rate limiting, schema validation, and Op… |
| `@reaatech/mcp-gateway-observability` | published v1.0.0 | Provides OpenTelemetry instrumentation, pre-configured metrics, and health check utilities for the MCP Gateway. It exports functions for registering custom health probes and ini… |
| `@reaatech/mcp-gateway-rate-limit` | published v1.0.0 | Enforces per-tenant rate limits using a token bucket algorithm with support for in-memory or Redis-backed storage. It provides an Express middleware function and a set of utilit… |
| `@reaatech/mcp-gateway-validation` | published v1.0.0 | Validates MCP protocol messages and custom tool arguments against JSON Schemas using AJV. It provides an Express middleware for JSON-RPC 2.0 request validation and a schema mana… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-gateway`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-gateway
- Browse packages: https://reaatech.com/products/mcp-infrastructure/mcp-gateway/packages
- npm scope: https://www.npmjs.com/~reaatech
