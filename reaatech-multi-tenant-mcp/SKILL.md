---
name: reaatech-multi-tenant-mcp
description: "These packages provide a suite of modular primitives for implementing multi-tenancy within Model Context Protocol (MCP) servers. They solve the challenge of isolating resources, enforcing rate limits, tracking costs, and managing configu…"
license: MIT
---

# REAA multi-tenant-mcp

These packages provide a suite of modular primitives for implementing multi-tenancy within Model Context Protocol (MCP) servers. They solve the challenge of isolating resources, enforcing rate limits, tracking costs, and managing configuration for different users within a single server instance. The collection is designed as a composable middleware pipeline, allowing you to wrap standard MCP request handlers with specific cross-cutting concerns like tenant resolution and visibility policies.

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
| `@reaatech/multi-tenant-mcp-artifact-store` | published v0.1.0 | Provides tenant-isolated artifact and file storage via `FileSystemArtifactStore` and `S3ArtifactStore` classes that implement the `ArtifactStore` interface (`put`, `get`, `list`, |
| `@reaatech/multi-tenant-mcp-config-isolation` | published v0.1.0 | Per-tenant configuration management with Zod schema validation, base-config merging, and versioned migrations; exports a `TenantConfigManager` facade backed by pluggable stores… |
| `@reaatech/multi-tenant-mcp-cost-accounting` | published v0.1.0 | Track per-tenant usage costs in a multi-tenant MCP server with per-call, per-token, and tiered pricing models. It provides `CostCalculator`, `CostTracker`, and `UsageEventEmitte… |
| `@reaatech/multi-tenant-mcp-middleware` | published v0.1.0 | Enforces multi-tenancy, rate limiting, and access control for Model Context Protocol (MCP) servers by providing a middleware factory that wraps request handlers. It requires the… |
| `@reaatech/multi-tenant-mcp-observability` | published v0.1.0 | Structured logging and per-tenant metrics for multi-tenant MCP servers, with log entries and counters |
| `@reaatech/multi-tenant-mcp-rate-limiter` | published v0.1.0 | Enforces per-tenant rate limits using a token-bucket algorithm via the `DefaultRateLimiter` class. It provides pluggable storage backends, including an LRU-bounded in-memory sto… |
| `@reaatech/multi-tenant-mcp-tenant-resolver` | published v0.1.0 | Resolve tenant identity from incoming MCP requests using headers, JWTs, or API keys, and propagate the resolved tenant context through `AsyncLocalStorage` via the provided resol… |
| `@reaatech/multi-tenant-mcp-tool-visibility` | published v0.1.0 | Provides a `VisibilityEngineImpl` class to restrict access to Model Context Protocol (MCP) tools, resources, and prompts based on tenant-specific allow-lists, deny-lists, or cus… |
| `@reaatech/multi-tenant-mcp-types` | published v0.1.0 | Shared |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-multi-tenant-mcp`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/multi-tenant-mcp
- Browse packages: https://reaatech.com/products/mcp-infrastructure/multi-tenant-mcp/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, llm, mcp, mcp-server, model-context-protocol, typescript
