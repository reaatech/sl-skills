---
name: reaatech-agent-budget-controller
description: "These packages provide a real-time enforcement layer for LLM agent systems, allowing you to define and monitor spending limits across users, sessions, and organizations. You would adopt them to prevent runaway costs by automatically trig…"
license: MIT
---

# REAA agent-budget-controller

These packages provide a real-time enforcement layer for LLM agent systems, allowing you to define and monitor spending limits across users, sessions, and organizations. You would adopt them to prevent runaway costs by automatically triggering warnings, downgrading models, or blocking requests before they execute. The system functions as a modular pipeline where a central engine coordinates state, pricing tables, and observability bridges to intercept and validate every LLM call.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Observability & Cost** category. 8 packages live under `@reaatech/agent-budget-cli` and siblings.

## Packages

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

## Quick start

```bash
npm install @reaatech/agent-budget-cli @reaatech/agent-budget-engine @reaatech/agent-budget-llm-router-plugin @reaatech/agent-budget-middleware @reaatech/agent-budget-otel-bridge @reaatech/agent-budget-pricing @reaatech/agent-budget-spend-tracker @reaatech/agent-budget-types
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-budget-controller`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-budget-controller
- Browse packages: https://reaatech.com/products/observability-cost/agent-budget-controller/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, budgets, llm, mcp, observability, openai, opentelemetry, typescript
