---
name: reaatech-mcp-gateway
description: "These packages provide a modular gateway framework for managing Model Context Protocol (MCP) infrastructure, including authentication, rate limiting, schema validation, and fan-out routing. They allow you to secure, observe, and scale co…"
license: MIT
---

# REAA mcp-gateway

These packages provide a modular gateway framework for managing Model Context Protocol (MCP) infrastructure, including authentication, rate limiting, schema validation, and fan-out routing. They allow you to secure, observe, and scale connections to upstream MCP servers by composing individual Express-compatible middleware components. The collection is designed as a set of independently versioned packages that share a unified configuration schema and core type system, enabling you to build a custom gateway or deploy the pre-configured server.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **MCP Infrastructure** category. 10 packages live under `@reaatech/mcp-gateway-allowlist` and siblings.

## Packages

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

## Quick start

```bash
npm install @reaatech/mcp-gateway-allowlist @reaatech/mcp-gateway-audit @reaatech/mcp-gateway-auth @reaatech/mcp-gateway-cache @reaatech/mcp-gateway-core @reaatech/mcp-gateway-fanout @reaatech/mcp-gateway-gateway @reaatech/mcp-gateway-observability @reaatech/mcp-gateway-rate-limit @reaatech/mcp-gateway-validation
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-gateway`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-gateway
- Browse packages: https://reaatech.com/products/mcp-infrastructure/mcp-gateway/packages
- npm scope: https://www.npmjs.com/~reaatech
