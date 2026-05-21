---
name: reaatech-multi-tenant-mcp
description: "These packages provide a suite of modular primitives for implementing multi-tenancy within Model Context Protocol (MCP) servers. They solve the challenge of isolating resources, enforcing rate limits, tracking costs, and managing configu…"
license: MIT
---

# REAA multi-tenant-mcp

These packages provide a suite of modular primitives for implementing multi-tenancy within Model Context Protocol (MCP) servers. They solve the challenge of isolating resources, enforcing rate limits, tracking costs, and managing configuration for different users within a single server instance. The collection is designed as a composable middleware pipeline, allowing you to wrap standard MCP request handlers with specific cross-cutting concerns like tenant resolution and visibility policies.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **MCP Infrastructure** category. 9 packages live under `@reaatech/multi-tenant-mcp-artifact-store` and siblings.

## Packages

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

## Quick start

```bash
npm install @reaatech/multi-tenant-mcp-artifact-store @reaatech/multi-tenant-mcp-config-isolation @reaatech/multi-tenant-mcp-cost-accounting @reaatech/multi-tenant-mcp-middleware @reaatech/multi-tenant-mcp-observability @reaatech/multi-tenant-mcp-rate-limiter @reaatech/multi-tenant-mcp-tenant-resolver @reaatech/multi-tenant-mcp-tool-visibility @reaatech/multi-tenant-mcp-types
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-multi-tenant-mcp`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/multi-tenant-mcp
- Browse packages: https://reaatech.com/products/mcp-infrastructure/multi-tenant-mcp/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, llm, mcp, mcp-server, model-context-protocol, typescript
