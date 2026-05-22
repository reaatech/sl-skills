---
name: reaatech-llm-cost-telemetry
description: "These packages give you drop-in wrappers for OpenAI, Anthropic, and Google SDKs that automatically capture token usage and cost, along with a cost calculation engine, multi-tenant aggregation,"
license: MIT
---

# REAA llm-cost-telemetry

These packages give you drop-in wrappers for OpenAI, Anthropic, and Google SDKs that automatically capture token usage and cost, along with a cost calculation engine, multi-tenant aggregation,

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Observability & Cost** category. 8 packages live under `@reaatech/llm-cost-telemetry` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

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
