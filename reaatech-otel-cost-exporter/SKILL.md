---
name: reaatech-otel-cost-exporter
description: "These packages convert GenAI semantic convention spans into real-time USD cost metrics and export them via Prometheus, OTLP, or JSON. They solve the problem of tracking LLM spend without maintaining pricing data, shipping with pre‑valida…"
license: MIT
---

# REAA otel-cost-exporter

These packages convert GenAI semantic convention spans into real-time USD cost metrics and export them via Prometheus, OTLP, or JSON. They solve the problem of tracking LLM spend without maintaining pricing data, shipping with pre‑validated pricing tables for major providers that are updated on patch releases. The packages compose as an OTel‑native pipeline—either as an in‑process SpanProcessor for the Node.js SDK or a standalone collector—and process only metadata, never LLM content.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Observability & Cost** category. 5 packages live under `@reaatech/otel-cost-exporter` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/otel-cost-exporter` | published v0.1.0 | Calculates real-time LLM usage costs by processing OpenTelemetry GenAI semantic convention spans. It provides a `SpanProcessor` for integration into Node.js OTel SDKs or a stand… |
| `@reaatech/otel-cost-exporter-calculator` | published v0.1.0 | Calculates GenAI token costs in USD by normalizing model names and processing token counts from OpenTelemetry semantic convention spans. It provides a `CostCalculator` class tha… |
| `@reaatech/otel-cost-exporter-cli` | published v0.1.0 | @reaatech/otel-cost-exporter-cli is a CLI that provides commands to |
| `@reaatech/otel-cost-exporter-core` | published v0.1.0 | Provides shared TypeScript types, Zod schemas, and GenAI semantic conventions for modeling and validating LLM cost data. It serves as the foundational library for the otel-cost-… |
| `@reaatech/otel-cost-exporter-pricing` | published v0.1.0 | Provides a lookup interface for LLM token pricing across major providers using bundled, versioned YAML data. It exports functions to load these datasets and instantiate a `Prici… |

## Quick start

```bash
npm install @reaatech/otel-cost-exporter @reaatech/otel-cost-exporter-calculator @reaatech/otel-cost-exporter-cli @reaatech/otel-cost-exporter-core @reaatech/otel-cost-exporter-pricing
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-otel-cost-exporter`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/otel-cost-exporter
- Browse packages: https://reaatech.com/products/observability-cost/otel-cost-exporter/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, anthropic, aws, devops, llm, observability, openai, opentelemetry, typescript
