---
name: reaatech-agent-handoff-protocol
description: "These packages give you a complete lifecycle for transferring a conversation from one AI agent to another mid-session, including compressing the conversation history, scoring and selecting the best target agent, validating payload compat…"
license: MIT
---

# REAA agent-handoff-protocol

These packages give you a complete lifecycle for transferring a conversation from one AI agent to another mid-session, including compressing the conversation history, scoring and selecting the best target agent, validating payload compatibility, delivering the handoff via MCP or A2A transport, and handling rejection with fallback alternatives. You would adopt them to solve the problem of routing a live multi-turn conversation between specialized agents without losing context or requiring the user to repeat themselves. The most distinctive thing is that every stage—compression, routing, validation, transport, and rejection handling—is a separate, pluggable package with zero runtime dependencies, so you can compose only the pieces you need and inject your own implementations for any stage.

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
npm install @reaatech/agent-handoff-compression @reaatech/agent-handoff-protocol @reaatech/agent-handoff-routing @reaatech/agent-handoff-transport @reaatech/agent-handoff-validation
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-handoff-compression` | published v0.1.0 | Compresses conversation history before agent handoff using three built-in strategies (hybrid, summary, sliding-window) with configurable token budgets. Exports compressor classe… |
| `@reaatech/agent-handoff-protocol` | published v0.1.0 | A TypeScript library for transferring a conversation from one AI agent to another mid-session, providing context compression, capability-based routing, payload validation, trans… |
| `@reaatech/agent-handoff-routing` | published v0.1.0 | A weighted scoring engine that selects the best target agent during a handoff, implementing a route/clarify/fallback decision tree with an in-memory agent registry. Exports `Cap… |
| `@reaatech/agent-handoff-transport` | published v0.1.0 | Transport layer implementations for delivering handoffs between agents, providing MCP (tool-call-based), A2A (HTTP POST with retry), and a transport factory with health-check ca… |
| `@reaatech/agent-handoff-validation` | published v0.1.0 | Validates `HandoffPayload` structure and checks agent compatibility (language, capacity, availability, history size) for the Agent Handoff Protocol. Exports a `HandoffValidator`… |
| `@reaatech/agent-handoff` | pending npm | Core types, utilities, and configuration for the Agent Handoff Protocol, providing 35+ TypeScript types, 7 typed error classes, a typed event emitter, a retry utility with confi… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-handoff-protocol`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-handoff-protocol
- Browse packages: https://reaatech.com/products/orchestration-protocols/agent-handoff-protocol/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, llm, mcp, model-context-protocol, multi-agent, typescript
