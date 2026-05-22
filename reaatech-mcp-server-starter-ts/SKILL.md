---
name: reaatech-mcp-server-starter-ts
description: "These packages provide a framework for building, securing, and operating Model Context Protocol (MCP) servers in TypeScript. They solve the complexity of implementing transport layers, authentication, and observability by offering a pre-…"
license: MIT
---

# REAA mcp-server-starter-ts

These packages provide a framework for building, securing, and operating Model Context Protocol (MCP) servers in TypeScript. They solve the complexity of implementing transport layers, authentication, and observability by offering a pre-configured Express-based engine with built-in middleware. The collection is designed as a modular pipeline where tools are auto-discovered from the filesystem and automatically integrated into a unified request-handling flow.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **MCP Infrastructure** category. 6 packages live under `@reaatech/mcp-server-auth` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/mcp-server-auth @reaatech/mcp-server-core @reaatech/mcp-server-engine @reaatech/mcp-server-observability @reaatech/mcp-server-tools @reaatech/mcp-server-transport
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-server-auth` | published v1.0.0 | Express middleware that validates API key or Bearer token headers on MCP server requests using constant-time comparison, with a development-mode bypass when no key is configured. |
| `@reaatech/mcp-server-core` | published v1.0.0 | Shared domain types (ToolResponse, ContentBlock, RequestContext), Zod schemas for runtime validation, environment configuration with cached env vars, and content-block helpers (… |
| `@reaatech/mcp-server-engine` | published v1.0.0 | Provides a framework for building MCP (Model Context Protocol) servers on top of Express 5, offering `createApp()` and `startServer()` |
| `@reaatech/mcp-server-observability` | published v1.0.0 | Provides structured logging, OpenTelemetry tracing, and Prometheus-compatible metrics for Model Context Protocol (MCP) servers. It exports a pre-configured Pino logger, span man… |
| `@reaatech/mcp-server-tools` | published v1.0.0 | Provides a type-safe `defineTool` helper and an in-memory registry for managing Model Context Protocol (MCP) tools. It includes filesystem auto-discovery for `.tool.ts` files an… |
| `@reaatech/mcp-server-transport` | published v1.0.0 | Mounts Model Context Protocol (MCP) transport handlers onto an Express application, providing session management and automatic cleanup for Streamable HTTP and SSE transports. It… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-server-starter-ts`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-server-starter-ts
- Browse packages: https://reaatech.com/products/mcp-infrastructure/mcp-server-starter-ts/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, aws, boilerplate, cloud-run, idempotency, jest, llm, mcp, mcp-server, model-context-protocol, observability, opentelemetry, prompt-injection, sse, starter, streamable-http, template, terraform, typescript, zod
