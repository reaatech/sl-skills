---
name: reaatech-llm-router
description: "These packages give you a config-driven LLM routing engine that selects models based on cost, latency, capability, or a two-tier judgment strategy, with automatic fallback chains and circuit breakers when models fail. You would adopt the…"
license: MIT
---

# REAA llm-router

These packages give you a config-driven LLM routing engine that selects models based on cost, latency, capability, or a two-tier judgment strategy, with automatic fallback chains and circuit breakers when models fail. You would adopt them to manage multi-provider LLM costs, enforce daily budgets, and add structured degradation paths without writing provider-specific orchestration code. The system is built as a set of independent packages—core types, engine, strategies, fallback, telemetry, MCP server, and CLI—that compose through a shared config schema and a single `executeModel` callback, keeping provider SDKs out of the router itself.

## When to use this

Reach for `llm-router` when the task involves sending LLM requests to multiple providers (e.g., OpenAI, Anthropic, Google, AWS Bedrock) and requires centralized control over *which* model handles each request. This family solves the “router or orchestrator” layer in an LLM-powered system: you define routing strategies, fallback chains, cost budgets, and observability hooks as configuration, and the engine handles execution.

**Trigger phrases** that indicate this family is the right choice:
- “Route this prompt to the cheapest model that can answer it.”
- “If OpenAI fails, fall back to Anthropic with retries.”
- “Set a monthly budget of $50 for all LLM calls.”
- “Balance latency and cost across different providers.”

The packages are designed for production workflows where you need resilience (circuit breakers, exponential backoff), cost enforcement (budget limits, per-request tracking), and strategic model selection (by cost, latency, capability, or judgment). The MCP package (`@reaatech/llm-router-mcp`) extends the router directly into AI agent tooling, letting agents decide which model to call via Model Context Protocol.

## Quick start example

```typescript
import { LLMRouter } from '@reaatech/llm-router-engine';
import { CostTrackingTelemetry } from '@reaatech/llm-router-telemetry';
import { LatencyOptimizedStrategy } from '@reaatech/llm-router-strategies';

const router = new LLMRouter({
  strategy: new LatencyOptimizedStrategy(),
  telemetry: new CostTrackingTelemetry({ monthlyBudget: 100 }),
  executeModel: async (model, prompt) => {
    // Call the provider SDK for `model` (e.g., OpenAI, Anthropic)
    return { text: '...', model, cost: 0.002, latencyMs: 450 };
  },
});

const result = await router.route({
  prompt: 'Explain quantum computing in simple terms',
  context: { maxCost: 0.01, preferredModels: ['gpt-4o', 'claude-3'] },
});
console.log(result.text, result.model, result.cost);
```

## Don’t reach for this when

- **You only ever call one provider and one model.** The router adds configuration overhead and a dependency on `llm-router-core` schemas. Use the provider’s SDK directly.
- **You need fine-grained per‑model features** (e.g., tool use schemas, image generation, streaming control beyond a simple callback). The router abstracts over providers; provider‑specific capabilities are not exposed — use the raw SDK for those.
- **You’re building a multi‑step agentic system** that requires state management, tool calls, and memory. Consider a dedicated agent orchestration library (e.g., LangChain, Vercel AI SDK, or another @reaatech agent framework).
- **You require real‑time streaming with backpressure or chunk‑based processing.** The `LLMRouter` returns a full response. For streaming, bypass the router and call the provider streaming API directly with your own middleware.
- **You need to train or fine‑tune models.** This family handles *routing* existing models, not training new ones. Use a training framework (e.g., Hugging Face Transformers, fine‑tuning APIs) instead.

## Packages

```bash
npm install @reaatech/llm-router-cli @reaatech/llm-router-core @reaatech/llm-router-engine @reaatech/llm-router-fallback @reaatech/llm-router-mcp @reaatech/llm-router-strategies @reaatech/llm-router-telemetry
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/llm-router-cli` | published v1.0.1 | A CLI that provides four commands—`route`, `benchmark`, `cost-report`, and `validate-config`—for routing prompts through configurable LLM strategies, benchmarking model performa… |
| `@reaatech/llm-router-core` | published v1.0.0 | Core TypeScript types, Zod schemas, and input validation for the llm-router ecosystem, providing 30+ domain types and 11 runtime validation schemas for model definitions, routin… |
| `@reaatech/llm-router-engine` | published v1.0.1 | A config-driven `LLMRouter` class that orchestrates model selection, fallback chains, cost tracking, A/B testing, quality scoring, and observability into a single routing decisi… |
| `@reaatech/llm-router-fallback` | published v1.0.0 | A function that creates ordered fallback chains for LLM calls, with per-model circuit breakers and exponential-backoff retry logic, returning a `FallbackChain` instance whose `e… |
| `@reaatech/llm-router-mcp` | published v1.0.0 | An MCP server that exposes three tools—`route_request`, `get_model_info`, and `get_cost_report`—for routing LLM requests through a pluggable router implementation via the Model… |
| `@reaatech/llm-router-strategies` | published v1.0.0 | A set of pluggable routing strategies (CostOptimized, LatencyOptimized, JudgmentBased, CapabilityBased) and a priority-based `StrategyOrchestrator` that evaluates them in order… |
| `@reaatech/llm-router-telemetry` | published v1.0.0 | A set of classes (`CostTracker`, `BudgetManager`, `CostReporter`) and a `MetricsCollector` facade for tracking per-request LLM costs, enforcing daily budget limits with configur… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-llm-router`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/llm-router
- Browse packages: https://reaatech.com/products/orchestration-protocols/llm-router/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai-infrastructure, anthropic, bedrock, cost-optimization, fallback, gemini, latency, llm, llm-router, llmops, model-routing, multi-model, ollama, openai, opentelemetry, routing-engine, typescript, vllm
