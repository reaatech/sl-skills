---
name: reaatech-mcp-gateway
description: "These packages give you a full-featured gateway for MCP (Model Context Protocol) servers, handling authentication, rate limiting, caching, schema validation, tool access control, and fan-out routing to multiple upstreams. You would adopt…"
license: MIT
---

# REAA mcp-gateway

These packages give you a full-featured gateway for MCP (Model Context Protocol) servers, handling authentication, rate limiting, caching, schema validation, tool access control, and fan-out routing to multiple upstreams. You would adopt them to add production middleware—like Kong or Envoy—in front of your MCP servers without building each piece from scratch. The ten packages are independently versioned and composable, so you can install only the middleware you need (e.g., just auth and rate limiting) or wire them all together through the provided Express 5 server with a CLI.

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
| `@reaatech/mcp-gateway-allowlist` | published v1.0.0 | A per-tenant tool access control library for MCP gateways, providing Express middleware that enforces allow/deny rules with wildcard pattern matching and versioned allowlist sto… |
| `@reaatech/mcp-gateway-audit` | published v1.0.0 | A structured audit logging library for the MCP Gateway that captures security-relevant events (auth, rate limiting, tool execution, cache operations) with configurable severity,… |
| `@reaatech/mcp-gateway-auth` | published v1.0.0 | Pluggable Express middleware that authenticates requests via API key, JWT (with JWKS), OAuth2 token introspection (RFC 7662), or OIDC ID token validation, attaching a typed `Aut… |
| `@reaatech/mcp-gateway-cache` | published v1.0.0 | An Express middleware and cache manager for MCP Gateway responses, providing in-memory LRU or Redis backends with per-tool TTL strategies, `Cache-Control` bypass support, and st… |
| `@reaatech/mcp-gateway-core` | published v1.0.0 | Core types, Zod schemas, configuration loading, and structured logging for the MCP Gateway ecosystem. It provides domain interfaces, runtime validation, YAML-based config loadin… |
| `@reaatech/mcp-gateway-fanout` | published v1.0.0 | A function that fans out a single MCP request to multiple upstream servers, then aggregates responses using strategies like first-success, all-wait, or majority-vote. It provide… |
| `@reaatech/mcp-gateway-gateway` | published v1.0.0 | An Express 5-based MCP Gateway server factory (`createApp()`) that wires together authentication, rate limiting, schema validation, tool allowlists, fan-out routing, response ca… |
| `@reaatech/mcp-gateway-observability` | published v1.0.0 | OpenTelemetry tracing, metrics, health checks, and structured logging for the MCP Gateway, providing auto-configured OTel SDK initialization, pre-built gateway metrics (counters… |
| `@reaatech/mcp-gateway-rate-limit` | published v1.0.0 | A per-tenant rate limiter for MCP gateways using a token bucket algorithm, providing Express middleware that enforces configurable per-minute and per-day request limits and sets… |
| `@reaatech/mcp-gateway-validation` | published v1.0.0 | JSON Schema validation for MCP protocol messages, providing an Express middleware that validates JSON-RPC 2.0 request structure and MCP method payloads, plus a `SchemaValidator`… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-gateway`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-gateway
- Browse packages: https://reaatech.com/products/mcp-infrastructure/mcp-gateway/packages
- npm scope: https://www.npmjs.com/~reaatech
