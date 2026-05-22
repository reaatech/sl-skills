---
name: reaatech-agent-handoff-protocol
description: "These packages provide a standardized lifecycle for transferring AI agent conversations, including context compression, intelligent routing, and transport delivery. You would adopt them to manage complex multi-agent workflows where sessi…"
license: MIT
---

# REAA agent-handoff-protocol

These packages provide a standardized lifecycle for transferring AI agent conversations, including context compression, intelligent routing, and transport delivery. You would adopt them to manage complex multi-agent workflows where sessions must move between specialized agents based on capability, load, and availability. The system is built as a modular, transport-agnostic protocol that allows you to swap compression strategies, routing logic, and transport layers like MCP or HTTP independently.

## When to use this

Reach for the `agent-handoff-protocol` family when the user describes a multi-agent system where conversations (or sessions) must move between different specialized AI agents. The core trigger is any explicit mention of "hand off", "transfer conversation", "pass context to another agent", "agent routing based on skill", or "load balancing between agents". Use it when the task requires compressing a long conversation history before sending it to another agent, or when you need a standardized lifecycle (compress → route → transport) that works over MCP or HTTP.

This family solves the problem of orchestrating agent-to-agent handoffs with configurable compression strategies and capability-aware routing. It is the right choice when the user says "I need to route a user session to the best agent based on expertise and current load", or "I want to compress chat history to stay under a token limit before passing it to a different agent". The protocol is transport-agnostic, so you can plug in different delivery mechanisms without changing the handoff logic.

Specific user requests that map here: "Set up a handoff pipeline between my QA agent and my code review agent", "Implement context-aware routing so a Spanish language query goes to the Spanish-speaking agent", or "Compress the last 50 messages into a summary before handoff". If the prompt contains words like "agent transfer", "session handoff", "context compression", or "capability router", this family applies.

## Quick start example

The example below creates a `HandoffManager` with a compression strategy, a capability-based router, and an MCP transport, then executes a handoff.

```typescript
import { HandoffManager } from '@reaatech/agent-handoff-protocol';
import { HandoffCompression } from '@reaatech/agent-handoff-compression';
import { CapabilityBasedRouter, AgentRegistry } from '@reaatech/agent-handoff-routing';
import { MCPTransport } from '@reaatech/agent-handoff-transport';

const registry = new AgentRegistry([{ id: 'agent-a', capabilities: ['code-review'] }]);
const router = new CapabilityBasedRouter(registry, { scoring: { load: 0.3, expertise: 0.7 } });
const compressor = new HandoffCompression({ strategy: 'extractive', maxTokens: 1000 });
const transport = new MCPTransport('http://localhost:3000/mcp');

const manager = new HandoffManager({ compressor, router, transport });
await manager.handoff({
  sessionId: 'session-1',
  conversation: [{ role: 'user', content: 'Review this PR' }],
  targetCapabilities: ['code-review'],
});
```

## Don't reach for this when

- You only have a single agent and no handoff requirement. Use direct API calls or a simple conversation loop instead.
- The handoff must happen synchronously inside a single request/response (e.g., within a single HTTP call). This family uses an asynchronous lifecycle with events; prefer a direct function that bundles all logic if synchronous behavior is mandatory.
- You already have a custom transport layer built into your agent infrastructure and don't want to depend on the provided MCP/HTTP implementations. In that case, implement the `HandoffTransport` interface from `@reaatech/agent-handoff` yourself, skipping the transport package.
- The conversation history is very short (<50 tokens) and doesn't need compression or routing. A simple direct handoff via a function call or a single-outgoing message is simpler.
- You need a fully deterministic, stateless chain of calls without event subscriptions. The protocol is designed for stateful, event-driven handoffs; if you just want to call agent B from agent A, use a direct tool call or RPC.

## Packages

```bash
npm install @reaatech/agent-handoff @reaatech/agent-handoff-compression @reaatech/agent-handoff-protocol @reaatech/agent-handoff-routing @reaatech/agent-handoff-transport @reaatech/agent-handoff-validation
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-handoff` | published v0.1.0 | Provides the core TypeScript definitions, error classes, and utility functions required to implement the Agent Handoff Protocol. It exports a set of interfaces, a typed event em… |
| `@reaatech/agent-handoff-compression` | published v0.1.0 | Reduces conversation history into a condensed format using sliding window, extractive summary, or hybrid strategies to fit within specific token budgets. It provides a set of co… |
| `@reaatech/agent-handoff-protocol` | published v0.1.0 | Orchestrates the transfer of conversation state between AI agents using a `HandoffManager` class that handles context compression, routing logic, and transport delivery. It prov… |
| `@reaatech/agent-handoff-routing` | published v0.1.0 | Selects the optimal agent for a handoff using a weighted scoring algorithm that evaluates skills, domain expertise, load, and language. It provides a `CapabilityBasedRouter` cla… |
| `@reaatech/agent-handoff-transport` | published v0.1.0 | Provides transport layer implementations for agent-to-agent handoffs via MCP tool calls or HTTP POST requests. It includes a factory class that performs health checks and auto-s… |
| `@reaatech/agent-handoff-validation` | published v0.1.0 | Validates Agent Handoff Protocol payloads and agent compatibility using a `HandoffValidator` class or standalone manual functions. It optionally integrates with Zod for schema e… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-handoff-protocol`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-handoff-protocol
- Browse packages: https://reaatech.com/products/orchestration-protocols/agent-handoff-protocol/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, llm, mcp, model-context-protocol, multi-agent, typescript
