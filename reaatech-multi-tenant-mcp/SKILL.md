---
name: reaatech-multi-tenant-mcp
description: "These packages let you run a single MCP server that serves multiple tenants, with each tenant getting its own rate limits, tool visibility, cost tracking, and isolated artifact storage. You'd adopt them to avoid standing up a separate MC…"
license: MIT
---

# REAA multi-tenant-mcp

These packages let you run a single MCP server that serves multiple tenants, with each tenant getting its own rate limits, tool visibility, cost tracking, and isolated artifact storage. You'd adopt them to avoid standing up a separate MCP server per customer while still enforcing per-tenant boundaries on usage, access, and billing. The distinctive thing is that every concern—tenant resolution, rate limiting, visibility, cost accounting, artifact storage, config isolation, and observability—is a separate package you compose into a single middleware function that wraps any MCP server's request handlers.

## When to use this

Reach for `multi-tenant-mcp` when you need to run a single MCP server that serves multiple distinct tenants — each with isolated configuration, rate limits, cost tracking, and tool availability. This family provides a composable middleware pipeline that plugs directly into an MCP request handler from `@modelcontextprotocol/sdk`. Use it when the prompt or task contains trigger phrases like: "multi-tenant MCP server", "isolate tenant data per user", "per-tenant rate limiting", "tenant-aware cost tracking", "filter tools by tenant", or "separate configs per organization".

The libraries work together as layered primitives: a tenant resolver extracts identity from headers/JWTs/api keys, a visibility engine filters tools/resources/prompts per tenant, rate limiters and cost trackers operate per tenant, and config isolation merges base config with tenant overrides. The `middleware` package wraps these into a single factory that you apply to your MCP server’s request handler. Use the whole family or pick individual packages — they are independent but designed to compose.

Typical users: teams building an MCP server that serves multiple clients (e.g., SaaS providers, platform teams) and need tenant-isolated resource access, usage billing, or compliance-driven configuration without running a separate server per tenant.

## Quick start example

```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { multiTenantMiddleware } from "@reaatech/multi-tenant-mcp-middleware";
import { HeaderTenantResolver } from "@reaatech/multi-tenant-mcp-tenant-resolver";
import { VisibilityEngineImpl } from "@reaatech/multi-tenant-mcp-tool-visibility";
import { DefaultRateLimiter } from "@reaatech/multi-tenant-mcp-rate-limiter";

const server = new Server({ name: "my-service", version: "1.0.0" });

const handler = multiTenantMiddleware({
  resolver: new HeaderTenantResolver({ header: "X-Tenant-Id" }),
  visibility: new VisibilityEngineImpl({
    tools: new Map([["tenant-a", ["read-user"]]])
  }),
  rateLimiter: new DefaultRateLimiter({ points: 100, duration: 60 })
});

server.setRequestHandler(handler);
```

This configures the MCP server to extract the tenant ID from an HTTP header, allow only the `read-user` tool for `tenant-a`, and enforce a per-tenant rate limit of 100 requests per minute.

## Don't reach for this when

- You need a simple single-tenant MCP server with no isolation concerns. Use `@modelcontextprotocol/sdk` directly without any middleware.
- You already run a separate MCP server instance per tenant (e.g., via Kubernetes namespaces). This family is for consolidating tenants into one server; if isolation requirements (security, compliance) demand process-level separation, keep separate instances.
- You need general-purpose authentication and authorization unrelated to MCP tool visibility. Look for a dedicated auth library (e.g., `@reaatech/auth-middleware` if applicable) or use a framework like Express/Passport outside the MCP layer.
- You require fine-grained row-level or field-level data visibility within tools (e.g., a single tool returning different fields per tenant). The visibility engine filters entire tools/resources/prompts, not data inside a tool’s response — combine with tenant-specific config inside the tool implementation.
- You are building an MCP client, not a server. This family targets server-side middleware; client-side tenant handling is out of scope.

## Packages

```bash
npm install @reaatech/multi-tenant-mcp-artifact-store @reaatech/multi-tenant-mcp-config-isolation @reaatech/multi-tenant-mcp-cost-accounting @reaatech/multi-tenant-mcp-middleware @reaatech/multi-tenant-mcp-observability @reaatech/multi-tenant-mcp-rate-limiter @reaatech/multi-tenant-mcp-tenant-resolver @reaatech/multi-tenant-mcp-tool-visibility @reaatech/multi-tenant-mcp-types
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/multi-tenant-mcp-artifact-store` | published v0.1.0 | Tenant-isolated artifact and file storage with automatic namespace prefixing, path traversal protection, and configurable per-tenant quotas. Provides `FileSystemArtifactStore` a… |
| `@reaatech/multi-tenant-mcp-config-isolation` | published v0.1.0 | A Zod-validated per-tenant configuration manager with base-config inheritance, pluggable storage, and versioned migration support. Exports `TenantConfigManager`, `ZodConfigValid… |
| `@reaatech/multi-tenant-mcp-cost-accounting` | published v0.1.0 | Provides interfaces and classes for per-tenant cost tracking in MCP servers, supporting per-call, per-token, and tiered discount pricing models with non-blocking usage event emi… |
| `@reaatech/multi-tenant-mcp-middleware` | published v0.1.0 | A function that wraps MCP server request handlers with a configurable pipeline for tenant resolution, rate limiting, tool/resource/prompt visibility filtering, cost accounting,… |
| `@reaatech/multi-tenant-mcp-observability` | published v0.1.0 | Provides structured console logging and LRU-bounded per-tenant metrics for multi-tenant MCP servers, exporting `ConsoleTenantLogger` and `MetricsCollector` classes that automati… |
| `@reaatech/multi-tenant-mcp-rate-limiter` | published v0.1.0 | A per-tenant rate limiter for MCP servers that enforces fixed-window request and token quotas, exposing a `DefaultRateLimiter` class backed by either an in-memory `MemoryRateLim… |
| `@reaatech/multi-tenant-mcp-tenant-resolver` | published v0.1.0 | Resolve tenant identity from incoming MCP requests via headers, JWTs, or API keys, and propagate it through `AsyncLocalStorage`. Exports resolver classes (`HeaderTenantResolver`… |
| `@reaatech/multi-tenant-mcp-tool-visibility` | published v0.1.0 | A class (`VisibilityEngineImpl`) and interface (`VisibilityEngine`) that control which MCP tools, resources, and prompts each tenant can see, supporting allow-list, deny-list, a… |
| `@reaatech/multi-tenant-mcp-types` | published v0.1.0 | Shared TypeScript types, error classes, and data structures for the multi-tenant MCP ecosystem, including `TenantContext`, `MiddlewareError`, `MiddlewareErrorCode`, and `Bounded… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-multi-tenant-mcp`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/multi-tenant-mcp
- Browse packages: https://reaatech.com/products/mcp-infrastructure/multi-tenant-mcp/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, llm, mcp, mcp-server, model-context-protocol, typescript
