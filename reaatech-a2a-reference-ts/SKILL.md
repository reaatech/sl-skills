---
name: reaatech-a2a-reference-ts
description: "These packages provide a complete TypeScript implementation of the Agent-to-Agent (A2A) protocol, including a server framework, client SDK, and a bidirectional bridge to the Model Context Protocol (MCP). You would adopt these to build in…"
license: MIT
---

# REAA a2a-reference-ts

These packages provide a complete TypeScript implementation of the Agent-to-Agent (A2A) protocol, including a server framework, client SDK, and a bidirectional bridge to the Model Context Protocol (MCP). You would adopt these to build interoperable AI agents that need to discover each other, manage task lifecycles, and exchange messages using standardized JSON-RPC and SSE streaming. The collection is built around a shared core of Zod schemas and pluggable adapters, allowing you to swap persistence layers, authentication strategies, and HTTP frameworks while maintaining strict type safety across the entire agent communication stack.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Orchestration & Protocols** category. 7 packages live under `@reaatech/a2a-reference-auth` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/a2a-reference-auth` | published v0.1.0 | Provides a set of interchangeable authentication strategies—including API key, JWT, and no-op—that implement a unified `authenticate` method. Each strategy is exported as a clas… |
| `@reaatech/a2a-reference-client` | published v0.1.0 | Provides a TypeScript client class for interacting with A2A agents via JSON-RPC 2.0 and Server-Sent Events. It includes built-in agent discovery, automatic retry logic, and an `… |
| `@reaatech/a2a-reference-core` | published v0.1.0 | Provides canonical TypeScript types, Zod schemas, and custom error classes for the Agent-to-Agent (A2A) protocol. It serves as a shared library for validating agent cards, tasks… |
| `@reaatech/a2a-reference-mcp-bridge` | published v0.1.0 | Exposes A2A agent skills as MCP tools and wraps MCP tools as A2A skills to enable interoperability between the two ecosystems. It provides classes for bidirectional protocol tra… |
| `@reaatech/a2a-reference-observability` | published v0.1.0 | Provides pre-configured Pino logger instances and utility functions for propagating correlation IDs across asynchronous boundaries. It exports a default logger and factory funct… |
| `@reaatech/a2a-reference-persistence` | published v0.1.0 | Provides a standardized `TaskStore` interface for persisting A2A task state, offering implementations for in-memory, file-system, and Redis storage. It supports paginated listin… |
| `@reaatech/a2a-reference-server` | published v0.1.0 | Provides Express and Hono adapters for building interoperable AI agents using JSON-RPC 2.0 routing, SSE streaming, and a task lifecycle state machine. It exposes factory functio… |

## Quick start

```bash
npm install @reaatech/a2a-reference-auth @reaatech/a2a-reference-client @reaatech/a2a-reference-core @reaatech/a2a-reference-mcp-bridge @reaatech/a2a-reference-observability @reaatech/a2a-reference-persistence @reaatech/a2a-reference-server
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-a2a-reference-ts`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/a2a-reference-ts
- Browse packages: https://reaatech.com/products/orchestration-protocols/a2a-reference-ts/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: a2a, agent-orchestration, agent-to-agent, agentic-ai, ai-agents, express, google-a2a, hono, llm-tools, mcp, mcp-bridge, model-context-protocol, monorepo, multi-agent, oauth2, opentelemetry, postgres, redis, sse, typescript
