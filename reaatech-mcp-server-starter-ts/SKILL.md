---
name: reaatech-mcp-server-starter-ts
description: "These packages give you a full MCP server—core types, an Express 5 framework with a composable middleware pipeline, tool registration and auto-discovery, dual Streamable HTTP and SSE transports, and built-in observability with Pino loggi…"
license: MIT
---

# REAA mcp-server-starter-ts

These packages give you a full MCP server—core types, an Express 5 framework with a composable middleware pipeline, tool registration and auto-discovery, dual Streamable HTTP and SSE transports, and built-in observability with Pino logging, OpenTelemetry tracing, and Prometheus metrics. You'd adopt them to build, secure, and operate an MCP server without wiring up authentication, rate limiting, idempotency, session management, or structured logging yourself. The most distinctive thing is that every concern—auth, rate limiting, idempotency, sanitization, tracing, metrics—is a separate, independently installable package that plugs into a single Express middleware pipeline, so you can compose exactly what you need.

## When to use this

Reach for this family when a task requires building, securing, and operating a Model Context Protocol (MCP) server in TypeScript. The most direct trigger is user mentions "MCP server" or "Model Context Protocol server" in any context that implies a production-grade setup: authentication, structured logging, OpenTelemetry tracing, Prometheus metrics, or tool auto-discovery. Other triggers include "build a tool server for AI agents", "secure MCP endpoints with API keys", or "add observability to an MCP service".

This family solves the entire lifecycle of an MCP server on Express 5. It replaces the need to wire up transport layers (Streamable HTTP, SSE), authentication middleware, tool registration, and observability plumbing yourself. The packages are designed as a modular pipeline: tools are discovered from the filesystem and automatically integrated into a unified request-handling flow. Use this when the user wants a drop-in framework that follows MCP specs without reinventing the stack.

Typical user shapes: a developer adding an MCP endpoint to an existing backend, a team building a standalone MCP server for AI agents, or a project that requires compliance-level logging and tracing (OpenTelemetry). If the prompt contains any of "MCP", "tool server", "AI agent server", or "Model Context Protocol", strongly consider this family.

## Quick start example

```typescript
import { createApp, startServer } from "@reaatech/mcp-server-engine";
import { defineTool, discoverTools } from "@reaatech/mcp-server-tools";
import { authMiddleware } from "@reaatech/mcp-server-auth";
import { registerTransports } from "@reaatech/mcp-server-transport";
import { initObservability } from "@reaatech/mcp-server-observability";

// Initialize observability (logging, tracing, metrics)
initObservability({ serviceName: "my-mcp-server" });

const app = createApp();
app.use(authMiddleware({ apiKey: process.env.MCP_API_KEY }));

// Auto-discover .tool.ts files and register them
discoverTools(defineTool);

// Mount MCP transports (Streamable HTTP / SSE)
registerTransports(app, { serverFactory: () => createMcpServer() });

startServer(app, { port: 3000 });
```

## Don't reach for this when

- **The user wants a single, one-off tool script, not a full server.** This family is for long-running, multi-tool MCP services. For a simple CLI that serves a single tool, use the raw `@modelcontextprotocol/sdk` directly with a local transport.
- **The server must run on a non-Express framework** (e.g., Fastify, Hono, AWS Lambda with API Gateway v2). The engine is built on Express 5. If the architecture requires a different HTTP framework, implement the MCP transport manually using `@modelcontextprotocol/sdk` and the chosen framework.
- **Authentication is not needed and the deployment is ephemeral** (e.g., a demo or prototype that doesn't leave localhost). The auth middleware adds overhead; skip this family and use `@modelcontextprotocol/sdk` with a simple HTTP server.
- **The transport must be raw WebSockets** (not SSE or Streamable HTTP). The `@reaatech/mcp-server-transport` only supports Streamable HTTP and SSE. For native WebSocket transport, build atop `@modelcontextprotocol/sdk` and a WebSocket library like `ws`.
- **Observability must use a different logging library or tracing exporter** (e.g., Winston, Datadog APM, or custom instrumentation). This family hard-wires Pino and OpenTelemetry with OTLP. If the project requires a specific vendor SDK or logging format, integrate those tools manually with `@modelcontextprotocol/sdk` instead.

## Packages

```bash
npm install @reaatech/mcp-server-auth @reaatech/mcp-server-core @reaatech/mcp-server-engine @reaatech/mcp-server-observability @reaatech/mcp-server-tools @reaatech/mcp-server-transport
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-server-auth` | published v1.0.1 | Express middleware that validates incoming MCP server requests via API key or Bearer token using constant-time comparison, with an automatic dev-mode bypass when no key is confi… |
| `@reaatech/mcp-server-core` | published v1.0.1 | Provides shared Zod schemas, domain types (`ToolResponse`, `ContentBlock`, `RequestContext`), validated environment configuration, and content block factories for the `@reaatech… |
| `@reaatech/mcp-server-engine` | published v1.0.3 | An Express 5-based MCP server framework that provides a composable middleware pipeline (auth, rate limiting, idempotency, sanitization), dual transport support (Streamable HTTP… |
| `@reaatech/mcp-server-observability` | published v1.1.0 | Provides structured logging (Pino-based with PII redaction), OpenTelemetry tracing with OTLP export, and Prometheus-compatible metrics for MCP servers, exporting a logger, `init… |
| `@reaatech/mcp-server-tools` | published v1.0.1 | A type-safe tool registry and discovery system for MCP servers, providing a `defineTool()` helper with Zod input schemas, an in-memory registry with `registerTool`/`getTools`/`g… |
| `@reaatech/mcp-server-transport` | published v1.2.0 | Mounts Streamable HTTP and SSE MCP transport handlers onto an Express application, managing session lifecycle, automatic cleanup, and transport-level metrics. Exports `mountStre… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-server-starter-ts`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-server-starter-ts
- Browse packages: https://reaatech.com/products/mcp-infrastructure/mcp-server-starter-ts/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, aws, boilerplate, cloud-run, idempotency, jest, llm, mcp, mcp-server, model-context-protocol, observability, opentelemetry, prompt-injection, sse, starter, streamable-http, template, terraform, typescript, zod
