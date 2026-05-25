---
name: reaatech-otel-cost-exporter
description: "These packages convert GenAI semantic convention spans into real-time USD cost metrics, exporting them via Prometheus, OTLP, or JSON. You would adopt them to track LLM spend per model and provider without manually maintaining pricing tab…"
license: MIT
---

# REAA otel-cost-exporter

These packages convert GenAI semantic convention spans into real-time USD cost metrics, exporting them via Prometheus, OTLP, or JSON. You would adopt them to track LLM spend per model and provider without manually maintaining pricing tables for OpenAI, Anthropic, Google, AWS Bedrock, and Azure. The packages compose as a layered pipeline—core types, pricing tables, a calculator with LRU caching, and an exporter that can run either as an in-process Node.js SpanProcessor or as a standalone OTLP collector.

## When to use this

Reach for this family when the user explicitly asks to **track, monitor, or export LLM token costs in real time** and already uses or plans to use OpenTelemetry for GenAI instrumentation. Trigger phrases like “monitor GenAI spend”, “cost per LLM request”, “track OpenAI/Anthropic costs in USD”, or “export LLM cost as Prometheus metric” map directly here. The family solves the coupling of LLM token counting to currency conversion by processing OpenTelemetry GenAI semantic convention spans (metadata only, never prompt/response content) and outputting USD cost metrics to Prometheus, OTLP, or JSON endpoints.

Also reach for this when the prompt describes a need to **calculate cost from existing GenAI spans** without maintaining a separate pricing database. The bundled, versioned pricing tables for major providers (OpenAI, Anthropic, AWS Bedrock, etc.) are updated on patch releases, so you avoid manual lookups. Common user requests like “add cost tracking to my LLM observability pipeline” or “convert OTel GenAI spans to dollars” are a direct fit. The family composes as an in-process `SpanProcessor` for the Node.js OTel SDK or as a standalone collector, letting you inject cost enrichment into your existing telemetry pipeline.

## Quick start example

The most common integration adds the cost exporter as an OpenTelemetry `SpanProcessor` on the Node.js SDK. After initializing the OTel SDK with GenAI instrumentation, create a `CostExporter` instance and register it. The exporter reads token counts from span attributes, applies the bundled pricing table, and exports cost metrics every 30 seconds by default.

```typescript
import { CostExporter } from '@reaatech/otel-cost-exporter';
import { NodeTracerProvider } from '@opentelemetry/sdk-trace-node';
import { SimpleSpanProcessor } from '@opentelemetry/sdk-trace-base';

const provider = new NodeTracerProvider();
provider.addSpanProcessor(
  new SimpleSpanProcessor(
    new CostExporter({
      endpoint: 'http://localhost:9090/api/v1/otlp',
      metricExportInterval: 30_000, // optional, default 30s
    })
  )
);
provider.register();
```

The exporter automatically converts spans with GenAI semantic attributes (e.g., `gen_ai.usage.completion_tokens`, `gen_ai.model`) into USD cost metrics like `llm.cost.usd`. No further configuration is needed for standard providers.

## Don’t reach for this when

- **You need cost data per user, session, or application tenant.** This family exports aggregate cost metrics (histograms, sums) by model and provider dimensions. For per‑user cost attribution, route span attributes into a separate metrics system or a custom exporter.
- **You want to record costs to a database or a billing system.** The exporter pushes metrics to Prometheus, OTLP, or JSON endpoints. If you need long‑term storage or invoice generation, ingest the metrics into a time‑series DB (e.g., Thanos, VictoriaMetrics) or use a webhook that transforms the JSON output.
- **You are not using OpenTelemetry for GenAI instrumentation.** The family only processes OpenTelemetry GenAI semantic spans. If your LLM calls are instrumented via a different framework (e.g., LangChain callbacks, manual OpenAI SDK logging), you must first convert them to OTel spans or use a direct cost SDK (e.g., `openai-cost-calc`).
- **You need to inspect or log the actual LLM request/response content for debugging.** This family strictly processes span metadata (model, token counts), never the content. If content auditing is required, pair with an OTel span exporter that includes `gen_ai.response.content` attributes.
- **Pricing tables must be externally managed or updated in near real time.** The pricing data is bundled and versioned with patch releases. For custom or dynamic pricing, implement your own `PricingTable` using the `@reaatech/otel-cost-exporter-pricing` interface, or fork the YAML data.

## Packages

```bash
npm install @reaatech/otel-cost-exporter @reaatech/otel-cost-exporter-calculator @reaatech/otel-cost-exporter-cli @reaatech/otel-cost-exporter-core @reaatech/otel-cost-exporter-pricing
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/otel-cost-exporter` | published v0.1.0 | An OpenTelemetry span processor that reads GenAI semantic convention spans, calculates real-time USD costs using bundled pricing tables for major LLM providers, and exports the… |
| `@reaatech/otel-cost-exporter-calculator` | published v0.1.0 | A cost calculator that converts raw token counts from GenAI semantic convention spans into per-model cost breakdowns in USD, with model name normalization, provider detection, a… |
| `@reaatech/otel-cost-exporter-cli` | published v0.1.0 | A CLI that starts an OTLP collector service, validates pricing tables, generates cost reports from span data, and inspects configuration, providing commands like `serve`, `repor… |
| `@reaatech/otel-cost-exporter-core` | published v0.1.0 | Shared domain types, Zod schemas, GenAI semantic convention constants, and a Pino-based logger factory for the `@reaatech/otel-cost-exporter-*` ecosystem. Exports TypeScript typ… |
| `@reaatech/otel-cost-exporter-pricing` | published v0.1.0 | A pricing table loader and lookup utility that bundles pre-validated YAML pricing data for OpenAI, Anthropic, Google, AWS Bedrock, and Azure, providing `loadPricingData()` and `… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-otel-cost-exporter`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/otel-cost-exporter
- Browse packages: https://reaatech.com/products/observability-cost/otel-cost-exporter/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, anthropic, aws, devops, llm, observability, openai, opentelemetry, typescript
