---
name: reaatech-agent-budget-controller
description: "These packages provide a real-time enforcement layer for LLM agent systems, allowing you to define and monitor spending limits across users, sessions, and organizations. You would adopt them to prevent runaway costs by automatically trig…"
license: MIT
---

# REAA agent-budget-controller

These packages provide a real-time enforcement layer for LLM agent systems, allowing you to define and monitor spending limits across users, sessions, and organizations. You would adopt them to prevent runaway costs by automatically triggering warnings, downgrading models, or blocking requests before they execute. The system functions as a modular pipeline where a central engine coordinates state, pricing tables, and observability bridges to intercept and validate every LLM call.

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
| `@reaatech/agent-budget-cli` | published v0.1.0 | Manages agent spending limits and budget configurations via a command-line interface. It provides commands to define budget caps, generate spend reports, and perform pre-flight… |
| `@reaatech/agent-budget-engine` | published v0.1.0 | Enforces LLM spending limits by providing a `BudgetController` class that manages state, performs pre-flight cost checks, and dynamically filters tools or downgrades models. It… |
| `@reaatech/agent-budget-llm-router-plugin` | published v0.1.0 | Filters LLM model candidates by remaining budget and blocks requests when limits are exceeded. It provides a `BudgetAwareStrategy` class that integrates with the LLM Router patt… |
| `@reaatech/agent-budget-middleware` | published v0.1.0 | Enforces LLM spending limits by providing Express/Fastify middleware and a `BudgetInterceptor` class that dynamically filters tools, downgrades models, and records token usage.… |
| `@reaatech/agent-budget-otel-bridge` | published v0.1.0 | Extracts GenAI usage metrics from OpenTelemetry span attributes to automatically record spend entries against budget scopes. It provides a `SpanListener` class that integrates w… |
| `@reaatech/agent-budget-pricing` | published v0.1.0 | Calculates LLM token costs and estimates budgets using a `PricingEngine` class that supports model name normalization and LRU-cached lookups. It includes built-in pricing tables… |
| `@reaatech/agent-budget-spend-tracker` | published v0.1.0 | Tracks and analyzes real-time spend data using an in-memory circular buffer for O(1) lookups and sliding-window aggregations. It provides a `SpendStore` class that enables cost… |
| `@reaatech/agent-budget-types` | published v0.1.0 | Provides TypeScript interfaces, Zod schemas, and error classes for defining and validating agent budget policies, enforcement actions, and spend tracking. It serves as the share… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-budget-controller`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-budget-controller
- Browse packages: https://reaatech.com/products/observability-cost/agent-budget-controller/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, budgets, llm, mcp, observability, openai, opentelemetry, typescript
