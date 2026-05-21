---
name: reaatech-agent-handoff-protocol
description: "These packages provide a standardized lifecycle for transferring AI agent conversations, including context compression, intelligent routing, and transport delivery. You would adopt them to manage complex multi-agent workflows where sessi…"
license: MIT
---

# REAA agent-handoff-protocol

These packages provide a standardized lifecycle for transferring AI agent conversations, including context compression, intelligent routing, and transport delivery. You would adopt them to manage complex multi-agent workflows where sessions must move between specialized agents based on capability, load, and availability. The system is built as a modular, transport-agnostic protocol that allows you to swap compression strategies, routing logic, and transport layers like MCP or HTTP independently.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Orchestration & Protocols** category. 6 packages live under `@reaatech/agent-handoff` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-handoff` | published v0.1.0 | Provides the core TypeScript definitions, error classes, and utility functions required to implement the Agent Handoff Protocol. It exports a set of interfaces, a typed event em… |
| `@reaatech/agent-handoff-compression` | published v0.1.0 | Reduces conversation history into a condensed format using sliding window, extractive summary, or hybrid strategies to fit within specific token budgets. It provides a set of co… |
| `@reaatech/agent-handoff-protocol` | published v0.1.0 | Orchestrates the transfer of conversation state between AI agents using a `HandoffManager` class that handles context compression, routing logic, and transport delivery. It prov… |
| `@reaatech/agent-handoff-routing` | published v0.1.0 | Selects the optimal agent for a handoff using a weighted scoring algorithm that evaluates skills, domain expertise, load, and language. It provides a `CapabilityBasedRouter` cla… |
| `@reaatech/agent-handoff-transport` | published v0.1.0 | Provides transport layer implementations for agent-to-agent handoffs via MCP tool calls or HTTP POST requests. It includes a factory class that performs health checks and auto-s… |
| `@reaatech/agent-handoff-validation` | published v0.1.0 | Validates Agent Handoff Protocol payloads and agent compatibility using a `HandoffValidator` class or standalone manual functions. It optionally integrates with Zod for schema e… |

## Quick start

```bash
npm install @reaatech/agent-handoff @reaatech/agent-handoff-compression @reaatech/agent-handoff-protocol @reaatech/agent-handoff-routing @reaatech/agent-handoff-transport @reaatech/agent-handoff-validation
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-handoff-protocol`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-handoff-protocol
- Browse packages: https://reaatech.com/products/orchestration-protocols/agent-handoff-protocol/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, llm, mcp, model-context-protocol, multi-agent, typescript
