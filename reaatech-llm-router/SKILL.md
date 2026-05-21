---
name: reaatech-llm-router
description: "These packages provide a centralized routing engine for managing LLM requests across multiple providers, balancing cost, latency, and model capabilities. They allow you to implement complex fallback chains, enforce strict budget limits,…"
license: MIT
---

# REAA llm-router

These packages provide a centralized routing engine for managing LLM requests across multiple providers, balancing cost, latency, and model capabilities. They allow you to implement complex fallback chains, enforce strict budget limits, and integrate LLM routing directly into agent workflows via the Model Context Protocol. The system is built around a configuration-driven architecture where routing strategies, observability hooks, and resilience patterns are composed into a single, unified execution pipeline.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Orchestration & Protocols** category. 7 packages live under `@reaatech/llm-router-cli` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/llm-router-cli` | published v1.0.0 | A command-line interface (CLI) for the `llm-router` package, exposing four commands—`route`, `benchmark`, `cost-report`, and `validate-config`—to send prompts |
| `@reaatech/llm-router-core` | published v1.0.0 | Provides the shared TypeScript interfaces, Zod schemas, and enums for defining LLM routing configurations, budget tracking, and circuit breaker states. It serves as the foundati… |
| `@reaatech/llm-router-engine` | published v1.0.0 | Provides an `LLMRouter` class that orchestrates model selection, fallback chains, cost tracking, and A/B testing for LLM requests. It requires a user-provided `executeModel` cal… |
| `@reaatech/llm-router-fallback` | published v1.0.0 | Implements resilience patterns for LLM API calls, providing a `FallbackChain` class that manages ordered model failover, circuit breaking, and exponential backoff retries. It re… |
| `@reaatech/llm-router-mcp` | published v1.0.0 | Exposes an MCP server (via `createMCPServer`) that provides three tools—`route_request`, `get_model_info`, and `get_cost_report`—allowing AI agents and orchestration frameworks… |
| `@reaatech/llm-router-strategies` | published v1.0.0 | Provides a collection of routing strategies and an orchestrator class to select the optimal LLM for a request based on cost, latency, capability, or judgment. It exposes a `Stra… |
| `@reaatech/llm-router-telemetry` | published v1.0.0 | Tracks LLM request costs and enforces budget limits through a set of utility classes for recording, aggregating, and reporting usage data. It provides an OpenTelemetry-compatibl… |

## Quick start

```bash
npm install @reaatech/llm-router-cli @reaatech/llm-router-core @reaatech/llm-router-engine @reaatech/llm-router-fallback @reaatech/llm-router-mcp @reaatech/llm-router-strategies @reaatech/llm-router-telemetry
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-llm-router`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/llm-router
- Browse packages: https://reaatech.com/products/orchestration-protocols/llm-router/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai-infrastructure, anthropic, bedrock, cost-optimization, fallback, gemini, latency, llm, llm-router, llmops, model-routing, multi-model, ollama, openai, opentelemetry, routing-engine, typescript, vllm
