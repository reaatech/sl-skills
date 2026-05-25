---
name: reaatech-agent-budget-controller
description: "These packages give you a real-time budget enforcement layer for LLM-powered agents that checks every request against per-task, per-user, per-session, or per-organization spend limits before it executes. You'd adopt them to prevent runaw…"
license: MIT
---

# REAA agent-budget-controller

These packages give you a real-time budget enforcement layer for LLM-powered agents that checks every request against per-task, per-user, per-session, or per-organization spend limits before it executes. You'd adopt them to prevent runaway agent loops from exhausting your LLM budget in minutes, with graceful degradation like model downgrades and tool filtering before a hard stop. The system is built as a set of composable packages—a core engine with a state machine, a circular-buffer spend tracker, pricing tables, and optional integrations for Express/Fastify middleware, an LLM Router plugin, and an OpenTelemetry bridge that automatically records GenAI spans as spend entries.

## When to use this

Reach for `@reaatech/agent-budget-controller` packages when you need to enforce LLM spending limits in real time before every inference call. The primary trigger is a user request that mentions **"set a budget for this agent"**, **"cap spend per session"**, **"warn me if costs exceed $X"**, or **"block expensive model calls"**. This family solves the problem of preventing cost overruns in multi-tenant or multi-agent systems where manual monitoring is impractical. It is designed to intercept LLM calls, evaluate remaining budget, and take configurable actions — downgrade to a cheaper model, filter expensive tools, or deny the request outright — before the call is dispatched.

The system is modular: you wire a `BudgetController` (from the engine) to a `SpendStore` (e.g., in-memory circular buffer from `spend-tracker`) and a `PricingEngine` (from the pricing package) that knows per-token costs. Integrations are provided as Express/Fastify middleware, a CLI for manual budget management, and an OpenTelemetry bridge that auto-records spend from GenAI span attributes. The types package gives you Zod schemas for policies and enforcement actions. You also get a plugin for LLM routers that filters candidates by remaining budget.

## Quick start example

```typescript
import { PricingEngine } from '@reaatech/agent-budget-pricing';
import { InMemorySpendStore } from '@reaatech/agent-budget-spend-tracker';
import { BudgetController } from '@reaatech/agent-budget-engine';

const pricing = new PricingEngine({ provider: 'openai' });
const store = new InMemorySpendStore({ windowMs: 3600000 });
const controller = new BudgetController({ pricing, store, maxSpend: 50 });

// Before each LLM call, check budget and get action
async function enforce(scope: string, model: string, inputTokens: number) {
  const cost = pricing.estimateCost(model, inputTokens);
  const action = controller.checkAndRecord(scope, cost);
  // action can be 'allow' | 'downgrade' | 'warn' | 'block'
  return action;
}
```

## Don't reach for this when

- **You only need passive cost tracking after the fact.** If the requirement is "log all LLM costs to a database for later analysis" without enforcement, use `@reaatech/otel-cost-exporter` (Observability & Cost family) or a custom OpenTelemetry exporter.
- **You want to enforce spending limits across multiple services or languages.** This family is TypeScript-only for Node.js. For polyglot enforcement, consider a sidecar proxy or an external rate-limiter like Redis-based token bucket.
- **You need fine-grained per-user budgets that require querying an external database.** The in-memory `SpendStore` is ephemeral. For persistent per-user spend, bring your own store (implement the `SpendStore` interface) backed by SQLite or PostgreSQL.
- **You are working inside a serverless function with very short-lived execution context.** Budget state is held in memory; for cold-start-sensitive environments, pre-budget or rely on a managed API gateway rate limit.

## Packages

```bash
npm install @reaatech/agent-budget-cli @reaatech/agent-budget-engine @reaatech/agent-budget-llm-router-plugin @reaatech/agent-budget-middleware @reaatech/agent-budget-otel-bridge @reaatech/agent-budget-pricing @reaatech/agent-budget-spend-tracker @reaatech/agent-budget-types
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-budget-cli` | published v0.1.0 | A CLI that installs a global `agent-budget` binary for managing agent budgets — defining budgets, checking remaining spend, listing active scopes, generating spend reports, simu… |
| `@reaatech/agent-budget-engine` | published v0.1.0 | A budget enforcement engine that provides pre-flight cost checks, real-time spend recording, and per-scope state machine transitions (Active → Warned → Degraded → Stopped) with… |
| `@reaatech/agent-budget-llm-router-plugin` | published v0.1.0 | A Fastify plugin that adds budget-aware routing to LLM Router, filtering model candidates by remaining budget and blocking requests when budgets are exhausted. It installs as th… |
| `@reaatech/agent-budget-middleware` | published v0.1.0 | Express/Fastify middleware and a direct SDK (`BudgetInterceptor`) that enforces per-scope budgets on agent requests by checking estimated cost, auto-downgrading models, filterin… |
| `@reaatech/agent-budget-otel-bridge` | published v0.1.0 | Converts OpenTelemetry GenAI spans into budget-tracked spend entries in real time by feeding span attributes to a `BudgetController`. Provides a `SpanListener` class that you ca… |
| `@reaatech/agent-budget-pricing` | published v0.1.0 | A pricing engine that computes LLM token costs using built-in or custom pricing tables, with LRU-cached lookups and model name normalization. Exports a `PricingEngine` class wit… |
| `@reaatech/agent-budget-spend-tracker` | published v0.1.0 | A circular-buffer-based in-memory spend tracker that records cost events and provides O(1) per-scope spend lookups, sliding-window rate calculations, cost projections, and anoma… |
| `@reaatech/agent-budget-types` | published v0.1.0 | Zod-validated TypeScript types, enums, and error classes defining budget scopes, policies, enforcement actions, spend entries, and state transitions for the agent-budget-control… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-budget-controller`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-budget-controller
- Browse packages: https://reaatech.com/products/observability-cost/agent-budget-controller/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, budgets, llm, mcp, observability, openai, opentelemetry, typescript
