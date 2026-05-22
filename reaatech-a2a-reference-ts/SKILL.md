---
name: reaatech-a2a-reference-ts
description: "These packages provide a complete TypeScript implementation of the Agent-to-Agent (A2A) protocol, including a server framework, client SDK, and a bidirectional bridge to the Model Context Protocol (MCP). You would adopt these to build in…"
license: MIT
---

# REAA a2a-reference-ts

These packages provide a complete TypeScript implementation of the Agent-to-Agent (A2A) protocol, including a server framework, client SDK, and a bidirectional bridge to the Model Context Protocol (MCP). You would adopt these to build interoperable AI agents that need to discover each other, manage task lifecycles, and exchange messages using standardized JSON-RPC and SSE streaming. The collection is built around a shared core of Zod schemas and pluggable adapters, allowing you to swap persistence layers, authentication strategies, and HTTP frameworks while maintaining strict type safety across the entire agent communication stack.

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
| `@reaatech/a2a-reference-auth` | published v0.1.0 | Provides a set of interchangeable authentication strategies—including API key, JWT, and no-op—that implement a unified `authenticate` method. Each strategy is exported as a clas… |
| `@reaatech/a2a-reference-client` | published v0.1.0 | Provides a TypeScript client class for interacting with A2A agents via JSON-RPC 2.0 and Server-Sent Events. It includes built-in agent discovery, automatic retry logic, and an `… |
| `@reaatech/a2a-reference-core` | published v0.1.0 | Provides canonical TypeScript types, Zod schemas, and custom error classes for the Agent-to-Agent (A2A) protocol. It serves as a shared library for validating agent cards, tasks… |
| `@reaatech/a2a-reference-mcp-bridge` | published v0.1.0 | Exposes A2A agent skills as MCP tools and wraps MCP tools as A2A skills to enable interoperability between the two ecosystems. It provides classes for bidirectional protocol tra… |
| `@reaatech/a2a-reference-observability` | published v0.1.0 | Provides pre-configured Pino logger instances and utility functions for propagating correlation IDs across asynchronous boundaries. It exports a default logger and factory funct… |
| `@reaatech/a2a-reference-persistence` | published v0.1.0 | Provides a standardized `TaskStore` interface for persisting A2A task state, offering implementations for in-memory, file-system, and Redis storage. It supports paginated listin… |
| `@reaatech/a2a-reference-server` | published v0.1.0 | Provides Express and Hono adapters for building interoperable AI agents using JSON-RPC 2.0 routing, SSE streaming, and a task lifecycle state machine. It exposes factory functio… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-a2a-reference-ts`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/a2a-reference-ts
- Browse packages: https://reaatech.com/products/orchestration-protocols/a2a-reference-ts/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: a2a, agent-orchestration, agent-to-agent, agentic-ai, ai-agents, express, google-a2a, hono, llm-tools, mcp, mcp-bridge, model-context-protocol, monorepo, multi-agent, oauth2, opentelemetry, postgres, redis, sse, typescript
