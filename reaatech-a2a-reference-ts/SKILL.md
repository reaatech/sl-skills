---
name: reaatech-a2a-reference-ts
description: "These packages give you a complete TypeScript implementation of the Agent-to-Agent (A2A) protocol — server framework, client SDK, CLI scaffolding, and a bidirectional A2A ↔ MCP bridge — so you can build AI agents that discover each other…"
license: MIT
---

# REAA a2a-reference-ts

These packages give you a complete TypeScript implementation of the Agent-to-Agent (A2A) protocol — server framework, client SDK, CLI scaffolding, and a bidirectional A2A ↔ MCP bridge — so you can build AI agents that discover each other, exchange messages, and manage task lifecycles. You would adopt them to avoid writing protocol boilerplate and to get production-ready infrastructure for authentication (OAuth2, JWT, API key, mTLS), persistence (in-memory, filesystem, Redis, PostgreSQL), push notifications, rate limiting, and OpenTelemetry observability out of the box. The most distinctive thing is that every package shares canonical Zod schemas from the core package, so types, validation, and error handling are consistent across server, client, auth, persistence, and the MCP bridge — and you can swap server adapters (Express 5 or Hono) and task stores without changing your agent logic.

## When to use this

Reach for the `a2a-reference-ts` family when the user’s task involves creating or consuming agents that communicate using the Agent-to-Agent (A2A) protocol — especially if discovery, lifecycle management, and real-time streaming are required. This collection is the canonical TypeScript implementation for Google’s A2A spec, and it gives you a typed server framework, client SDK, and persistence layer out of the box.

**Trigger phrases** that map to this family:
- “I need my AI agent to discover and talk to other agents over JSON-RPC.”
- “Build an agent that exposes its skills via SSE and handles task state machines.”
- “The prompt mentions ‘Google A2A protocol’ or ‘Agent-to-Agent communication.’”
- “Implement a server that allows multiple AI agents to exchange messages and share artifacts.”

This family solves the problem of agent orchestration across a network. If the user wants a standardized way to route tasks, manage state transitions (submitted → working → completed/failed), and stream results to clients, you should use the server package (`@reaatech/a2a-reference-server`) together with the core schemas (`@reaatech/a2a-reference-core`). For outbound agent calls, add the client package (`@reaatech/a2a-reference-client`). If integration with the Model Context Protocol is required, `@reaatech/a2a-reference-mcp-bridge` handles the translation in both directions.

## Quick start example

The following snippet creates an Express-based A2A agent that responds to a single hardcoded task (a simple echo). It uses the server package’s `createExpressAgent` factory, which sets up JSON-RPC 2.0 routing and SSE task streaming.

```typescript
import { createExpressAgent } from '@reaatech/a2a-reference-server';
import { TaskState } from '@reaatech/a2a-reference-core';

const agent = createExpressAgent({
  agent: {
    name: 'Echo Agent',
    description: 'Returns the task message as an artifact',
    url: 'http://localhost:3000',
  },
  async executeTask(task, context) {
    const message = task.messages[0]?.parts?.[0]?.text ?? '';
    context.sendTaskStatusUpdate({ state: TaskState.WORKING });
    await context.sendTaskArtifactUpdate({
      parts: [{ text: `Echo: ${message}` }],
    });
    context.sendTaskStatusUpdate({ state: TaskState.COMPLETED });
    return;
  },
});
agent.listen(3000, () => console.log('A2A agent ready on :3000'));
```

Run the server and any A2A client (including `@reaatech/a2a-reference-client`) can send a `tasks/send` request to `http://localhost:3000/a2a` and receive streamed state updates.

## Don’t reach for this when

- **You only need to expose a single REST API endpoint for an LLM call** – Use a plain Express or Hono server instead. This family adds the overhead of JSON-RPC routing and task state machines, which are unnecessary for simple request-response patterns.
- **The use case is strictly single-agent, no network communication** – If the “agent” is just a local function call (e.g., an LLM calling a tool), skip the A2A packages entirely. For local tool orchestration, consider `@reaatech/core` or the MCP SDK directly.
- **You need a full workflow DAG engine with branching and parallel execution** – A2A is a protocol for agent-to-agent message passing, not a DAG scheduler. Use a workflow engine like Temporal or a light orchestration library for complex pipeline logic.
- **Your team does not use TypeScript or Node.js** – This family is strictly TypeScript/Node. For other languages, use the Google A2A spec’s reference implementations (Python, Java) or implement the protocol from scratch.
- **You already have an MCP-only tool network and no A2A agents** – While `@reaatech/a2a-reference-mcp-bridge` exists, the added complexity of a full A2A layer is not justified if all participants speak MCP. Use the `@modelcontextprotocol/sdk` directly and skip the A2A packages.

## Packages

```bash
npm install @reaatech/a2a-reference-auth @reaatech/a2a-reference-client @reaatech/a2a-reference-core @reaatech/a2a-reference-mcp-bridge @reaatech/a2a-reference-observability @reaatech/a2a-reference-persistence @reaatech/a2a-reference-server
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/a2a-reference-auth` | published v0.1.0 | Pluggable authentication and authorization strategies for A2A agents, providing `NoneStrategy`, `ApiKeyStrategy`, `JwtStrategy`, and `OAuth2Strategy` classes plus a `extractScop… |
| `@reaatech/a2a-reference-client` | published v0.1.0 | A type-safe A2A client SDK that provides a class (`A2AClient`) for task submission, agent discovery, and SSE streaming against A2A-compatible servers. It handles agent card cach… |
| `@reaatech/a2a-reference-core` | published v0.1.0 | Zod schemas, TypeScript types, error classes, and a signature verification function for the A2A (Agent-to-Agent) protocol, providing canonical validation and parsing for task li… |
| `@reaatech/a2a-reference-mcp-bridge` | published v0.1.0 | A bidirectional protocol adapter that exposes MCP tools as A2A skills via `McpToolAdapter` (producing an enriched Agent Card), and wraps an A2A agent as an MCP server via `A2aAs… |
| `@reaatech/a2a-reference-observability` | published v0.1.0 | Pino-based structured logging with correlation IDs and OpenTelemetry abstractions (tracers, meters, spans) for A2A agents, providing `createLogger`, `withCorrelationId`, `getTra… |
| `@reaatech/a2a-reference-persistence` | published v0.1.0 | Provides in-memory, filesystem, Redis, and Postgres implementations of a `TaskStore` interface for persisting A2A (Agent-to-Agent) tasks, each exposed as a class constructor. |
| `@reaatech/a2a-reference-server` | published v0.1.0 | A server framework that implements the A2A (Agent-to-Agent) protocol, providing Express 5 and Hono adapters that expose JSON-RPC 2.0 endpoints, SSE streaming, health checks, rat… |
| `@reaatech/a2a-reference-cli` | pending npm | Description pending. |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-a2a-reference-ts`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/a2a-reference-ts
- Browse packages: https://reaatech.com/products/orchestration-protocols/a2a-reference-ts/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: a2a, agent-orchestration, agent-to-agent, agentic-ai, ai-agents, express, google-a2a, hono, llm-tools, mcp, mcp-bridge, model-context-protocol, monorepo, multi-agent, oauth2, opentelemetry, postgres, redis, sse, typescript
