---
name: reaatech-llm-cost-telemetry
description: "These packages give you drop-in wrappers for OpenAI, Anthropic, and Google SDKs that automatically capture token usage and cost, along with a cost calculation engine, multi-tenant aggregation,"
license: MIT
---

# REAA llm-cost-telemetry

These packages give you drop-in wrappers for OpenAI, Anthropic, and Google SDKs that automatically capture token usage and cost, along with a cost calculation engine, multi-tenant aggregation,

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
| `@reaatech/llm-cost-telemetry` | published v0.1.0 | Provides shared TypeScript types, Zod schemas, and utility functions for modeling, validating, and calculating LLM cost telemetry data. It serves as the foundational library for… |
| `@reaatech/llm-cost-telemetry-aggregation` | published v0.1.0 | Provides buffered cost span collection, multi-dimensional aggregation by tenant/feature/route/model, and per-tenant budget enforcement with cascading alert thresholds via three… |
| `@reaatech/llm-cost-telemetry-calculator` | published v0.1.0 | A provider-agnostic cost calculation engine for LLM API usage with cache-aware pricing, token counting (tiktoken-based for OpenAI, estimates for others), and pre-call estimation… |
| `@reaatech/llm-cost-telemetry-cli` | published v0.1.0 | A CLI tool that reads LLM cost span data from |
| `@reaatech/llm-cost-telemetry-exporters` | published v0.1.0 | Provides exporter classes (CloudWatchExporter |
| `@reaatech/llm-cost-telemetry-mcp` | published v0.1.0 | Exposes LLM cost tracking, aggregation, and budget enforcement as a set of Model Context Protocol (MCP) tools. It provides a factory function that returns an MCP server instance… |
| `@reaatech/llm-cost-telemetry-observability` | published v0.1.0 | Provides structured OpenTelemetry tracing and metrics for monitoring LLM costs, plus Pino-based logging with PII redaction, exposed as `TracingManager`, `MetricsManager`, and `g… |
| `@reaatech/llm-cost-telemetry-providers` | published v0.1.0 | Wraps official OpenAI, Anthropic, and Google Generative AI SDK clients to automatically capture token usage, latency, and custom metadata for every API call. It provides wrapper… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-llm-cost-telemetry`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/llm-cost-telemetry
- Browse packages: https://reaatech.com/products/observability-cost/llm-cost-telemetry/packages
- npm scope: https://www.npmjs.com/~reaatech
