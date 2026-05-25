---
name: reaatech-otel-genai-semconv
description: "These packages give you instrumented wrappers for OpenAI, Anthropic, Vertex AI, and AWS Bedrock that emit OpenTelemetry GenAI semantic convention spans, plus deployable dashboards for Phoenix, Langfuse, and Cloud Trace. You would adopt t…"
license: MIT
---

# REAA otel-genai-semconv

These packages give you instrumented wrappers for OpenAI, Anthropic, Vertex AI, and AWS Bedrock that emit OpenTelemetry GenAI semantic convention spans, plus deployable dashboards for Phoenix, Langfuse, and Cloud Trace. You would adopt them to get spec-compliant observability across multiple LLM providers without writing instrumentation code yourself. The packages are designed as independent, installable modules—core types, instrumentation framework, provider wrappers, utilities, and exporters—so you can compose exactly what you need rather than pulling in a monolithic library.

## When to use this

Reach for `otel-genai-semconv` when a task requires capturing standardized OpenTelemetry spans from LLM provider SDKs — OpenAI, Anthropic, Bedrock, or Vertex AI — and piping them into an existing OTel pipeline, Arize Phoenix, Langfuse, or Google Cloud Trace. Trigger phrases include: “instrument OpenAI with OpenTelemetry”, “track token usage and cost per LLM call”, “send gen_ai spans to Phoenix”, “redact PII from LLM request payloads”, “aggregate streaming metrics across providers”, or “add circuit breaking to Anthropic client”. The family solves the observability gap for LLM interactions by wrapping SDK clients with zero-config wrappers that emit semantically compliant spans, estimate token counts, calculate costs from provider pricing, and redact sensitive fields — all without changing existing application logic.

Use these packages when you need a consistent, provider-agnostic telemetry layer across multiple AI models. The shared instrumentation framework in `@reaatech/otel-genai-semconv-instrumentation` handles lifecycle hooks, retries, and error classification, while the exporter package (`@reaatech/otel-genai-semconv-exporters`) transforms `gen_ai.*` spans into native formats for backend systems like Arize Phoenix and Langfuse. If your prompt involves “trace GPT-4 and Claude calls in one dashboard”, “estimate cost before billing run”, or “see streaming token rates per request”, this family is the intended solution.

## Quick start example

The most common path is instrumenting the OpenAI client. After initializing the OpenTelemetry SDK, wrap an `OpenAI` instance with `OpenAIInstrumentation`. All subsequent `chat.completions.create` calls automatically emit spans with token usage and cost estimates.

```typescript
import { OpenAI } from 'openai';
import { OpenAIInstrumentation } from '@reaatech/otel-genai-semconv-openai';
import { initObservability } from '@reaatech/otel-genai-semconv-observability';

initObservability({ serviceName: 'llm-app' }); // sets up OTel SDK, metrics, and logging

const client = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });
const instrumented = new OpenAIInstrumentation(client);

// Every completion call now emits a gen_ai span with token usage and latency
const response = await instrumented.client.chat.completions.create({
  model: 'gpt-4o',
  messages: [{ role: 'user', content: 'Explain quantum computing like I’m 10.' }]
});
```

For Anthropic, replace with `@reaatech/otel-genai-semconv-anthropic`; for Bedrock, call `instrument()` from `@reaatech/otel-genai-semconv-bedrock` on the `BedrockRuntimeClient`.

## Don’t reach for this when

- You only need to **log raw request/response payloads for debugging** without OpenTelemetry traces. Use a structured logger (e.g., Pino directly) or provider-specific logging hooks instead.
- You want a **full vendor-specific dashboard** like OpenAI’s own Usage page or Anthropic’s Console — these packages emit OTel spans, not proprietary metrics endpoints. Use provider dashboards for provider-specific cost breakdowns.
- You require **distributed tracing across a monolith without LLM focus** — this family concentrates on `gen_ai` semantic conventions. For generic HTTP or database tracing, use standard `@opentelemetry/instrumentation-http` or `@opentelemetry/instrumentation-pg`.
- You need **real-time cost tracking inside a custom billing system** — the cost calculator (`@reaatech/otel-genai-semconv-utils`) estimates tokens and costs per call, but for exact billing-level aggregation use provider APIs or a dedicated cost management platform.
- You are working with a **non-supported provider** (e.g., Cohere, Mistral, Hugging Face) — this family only wraps OpenAI, Anthropic, Bedrock, and Vertex AI. For other providers, implement a custom instrumentor using `@reaatech/otel-genai-semconv-core` and the shared instrumentation framework.

## Packages

```bash
npm install @reaatech/otel-genai-semconv-anthropic @reaatech/otel-genai-semconv-bedrock @reaatech/otel-genai-semconv-core @reaatech/otel-genai-semconv-exporters @reaatech/otel-genai-semconv-instrumentation @reaatech/otel-genai-semconv-observability @reaatech/otel-genai-semconv-openai @reaatech/otel-genai-semconv-utils @reaatech/otel-genai-semconv-vertexai
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/otel-genai-semconv-anthropic` | published v0.1.0 | An OpenTelemetry instrumentation for the Anthropic Node.js SDK that wraps `client.messages.create()` to emit GenAI semantic convention spans with request metadata, token usage,… |
| `@reaatech/otel-genai-semconv-bedrock` | published v0.1.0 | A zero-config OpenTelemetry instrumentation class for the AWS Bedrock Runtime SDK that wraps `client.send()` to emit GenAI semantic convention spans with model-family-aware attr… |
| `@reaatech/otel-genai-semconv-core` | published v0.1.0 | Canonical TypeScript types, constants, Zod schemas, and span-builder utilities for the OpenTelemetry GenAI semantic conventions, providing the single source of truth for all `ge… |
| `@reaatech/otel-genai-semconv-exporters` | published v0.1.0 | Custom OpenTelemetry span exporters that convert GenAI spans into the native formats of Arize Phoenix, Langfuse, and Google Cloud Trace, each implementing the `SpanExporter` int… |
| `@reaatech/otel-genai-semconv-instrumentation` | published v0.1.0 | Provides tracer management, lifecycle hooks, streaming response handling, error classification, retry with exponential backoff, per-provider circuit breaking, chunk aggregation,… |
| `@reaatech/otel-genai-semconv-observability` | published v0.1.0 | A zero-config observability kit for GenAI workloads, providing structured Pino logging, OpenTelemetry SDK initialization with OTLP trace export, pre-built metrics counters/histo… |
| `@reaatech/otel-genai-semconv-openai` | published v0.1.0 | A function `instrument(client)` that wraps an OpenAI SDK client instance to automatically emit OpenTelemetry GenAI semantic convention spans for every `chat.completions.create()… |
| `@reaatech/otel-genai-semconv-utils` | published v0.1.0 | Token counting, cost calculation, and PII redaction utilities for LLM observability. Provides a `TokenCounter` class with tiktoken-based estimation for OpenAI models and charact… |
| `@reaatech/otel-genai-semconv-vertexai` | published v0.1.0 | A function that wraps a Google Generative Language (Vertex AI) SDK model instance to automatically emit OpenTelemetry GenAI semantic convention spans on every `generateContent()… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-otel-genai-semconv`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/otel-genai-semconv
- Browse packages: https://reaatech.com/products/observability-cost/otel-genai-semconv/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: ai-infrastructure, anthropic, arize-phoenix, bedrock, cloud-trace, genai, instrumentation, langfuse, llm, observability, openai, opentelemetry, otel, semantic-conventions, spans, tracing, typescript, vertex-ai
