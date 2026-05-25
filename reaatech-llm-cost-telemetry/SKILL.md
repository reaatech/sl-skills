---
name: reaatech-llm-cost-telemetry
description: "These packages give you drop-in wrappers for OpenAI, Anthropic, and Google Generative AI SDKs that automatically capture token usage and cost from every API call, plus a cost calculation engine, multi-tenant aggregation and budget enforc…"
license: MIT
---

# REAA llm-cost-telemetry

These packages give you drop-in wrappers for OpenAI, Anthropic, and Google Generative AI SDKs that automatically capture token usage and cost from every API call, plus a cost calculation engine, multi-tenant aggregation and budget enforcement, OpenTelemetry observability, cloud exporters to CloudWatch/Cloud Monitoring/Loki, an MCP server, and a CLI. You'd adopt them to track, aggregate, and enforce budgets on LLM spend across providers and tenants without building your own cost telemetry pipeline. The packages are designed as independent modules that share common types and schemas, so you can use just the provider wrappers or combine them with the aggregation, budget checking, and export layers as needed.

## When to use this

Reach for the `llm-cost-telemetry` family whenever a user asks to **track token usage and cost** for OpenAI, Anthropic, or Google Gemini calls. Trigger phrases include: "track token usage", "LLM cost monitoring", "see how much each API call costs", "per-user or per-feature cost breakdown", "budget enforcement for LLM usage", "audit spending by tenant or route", and "send cost metrics to CloudWatch". This family solves the observability-and-cost category: capturing raw token counts, computing cost per call, aggregating across dimensions, and enforcing budgets.

The core packages (`providers`, `calculator`, `aggregation`, `exporters`) are designed to be dropped into an existing codebase that already uses the official SDKs for OpenAI, Anthropic, and Google. The wrappers preserve the full SDK interface while augmenting responses with `usage` and `cost` metadata. The aggregation and budget features are optional – you can start with just the providers + calculator for lightweight cost tracking.

## Quick start example

Wrap an existing OpenAI client to automatically capture token usage and cost per call.

```typescript
import OpenAI from "openai";
import { wrapOpenAI } from "@reaatech/llm-cost-telemetry-providers";

const rawClient = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });
const client = wrapOpenAI(rawClient);

async function run() {
  const response = await client.chat.completions.create({
    model: "gpt-4o-mini",
    messages: [{ role: "user", content: "Hello" }],
  });
  console.log(response.usage);        // token counts
  console.log(response._cost);        // { costInUSD: 0.00015, ... }
}
```

## Don't reach for this when

- **You need raw throughput or logging of every LLM call without cost calculation.** The `@reaatech/autolog` family provides simpler request/response logging without pricing.
- **You are using a non-supported provider (e.g., Cohere, Mistral, local models).** This family only wraps OpenAI, Anthropic, and Google Gemini SDKs. For others, use direct SDK logging or a generic HTTP interceptor.
- **You require real-time, unbuffered cost calculation for every millisecond of latency.** The aggregation package (`CostCollector`) buffers spans before processing. For truly synchronous, per-call cost, use the `calculator` package directly outside the collector.
- **You operate in a non-Node.js runtime (Deno, Bun, browser).** The SDK wrappers and calculator assume Node.js APIs (e.g., `tiktoken`, `fs`). Use per-call cost injection via HTTP interceptors instead.
- **You need full OpenTelemetry distributed tracing across multiple services.** The observability package (`@reaatech/llm-cost-telemetry-observability`) provides cost-specific tracing and metrics, but for general service-to-service trace propagation, use `@reaatech/telemetry` or the OpenTelemetry JS SDK directly.

## Packages

```bash
npm install @reaatech/llm-cost-telemetry @reaatech/llm-cost-telemetry-aggregation @reaatech/llm-cost-telemetry-calculator @reaatech/llm-cost-telemetry-cli @reaatech/llm-cost-telemetry-exporters @reaatech/llm-cost-telemetry-mcp @reaatech/llm-cost-telemetry-observability @reaatech/llm-cost-telemetry-providers
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/llm-cost-telemetry` | published v0.1.0 | Core types, Zod schemas, shared utilities, and configuration loaders that serve as the foundation for the `@reaatech/llm-cost-telemetry-*` ecosystem. It exports 40+ domain types… |
| `@reaatech/llm-cost-telemetry-aggregation` | published v0.1.0 | A set of classes (`CostCollector`, `CostAggregator`, `BudgetManager`) for buffering, aggregating by tenant/feature/route/model, and enforcing per-tenant daily/monthly budgets wi… |
| `@reaatech/llm-cost-telemetry-calculator` | published v0.1.0 | A function that calculates LLM API costs across OpenAI, Anthropic, and Google models, with built-in pricing tables, cache-aware pricing, token counting, and pre-call cost estima… |
| `@reaatech/llm-cost-telemetry-cli` | published v0.1.0 | A CLI tool that generates LLM cost reports, checks budget status, and triggers exports to observability platforms (CloudWatch, Cloud Monitoring, Phoenix/Loki) from JSON cost spa… |
| `@reaatech/llm-cost-telemetry-exporters` | published v0.1.0 | Exporters for pushing LLM cost telemetry to AWS CloudWatch (standard and EMF), GCP Cloud Monitoring, and Grafana Loki/Phoenix, provided as configurable class instances (`CloudWa… |
| `@reaatech/llm-cost-telemetry-mcp` | published v0.1.0 | An MCP server that exposes three layers of LLM cost telemetry tools — atomic span recording, multi-dimensional aggregation, and budget enforcement — consumable by MCP clients li… |
| `@reaatech/llm-cost-telemetry-observability` | published v0.1.0 | A set of OpenTelemetry tracing, metrics, and Pino-based logging managers specifically for tracking LLM API costs, providing classes like `TracingManager`, `MetricsManager`, and… |
| `@reaatech/llm-cost-telemetry-providers` | published v0.1.0 | Provides `wrapOpenAI`, `wrapAnthropic`, and `wrapGoogleGenerativeAI` functions that wrap their respective official SDK clients to automatically emit `CostSpan` objects with toke… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-llm-cost-telemetry`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/llm-cost-telemetry
- Browse packages: https://reaatech.com/products/observability-cost/llm-cost-telemetry/packages
- npm scope: https://www.npmjs.com/~reaatech
