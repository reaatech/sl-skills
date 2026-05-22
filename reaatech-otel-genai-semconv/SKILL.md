---
name: reaatech-otel-genai-semconv
description: "These packages provide instrumented wrappers for OpenAI, Anthropic, Vertex AI, and AWS Bedrock that emit OpenTelemetry-compliant spans for LLM interactions. You would adopt them to gain standardized observability into token usage, costs,…"
license: MIT
---

# REAA otel-genai-semconv

These packages provide instrumented wrappers for OpenAI, Anthropic, Vertex AI, and AWS Bedrock that emit OpenTelemetry-compliant spans for LLM interactions. You would adopt them to gain standardized observability into token usage, costs, streaming performance, and error rates across multiple AI providers. The collection is built on a shared instrumentation framework that uses a zero-config wrapper pattern to inject lifecycle hooks, circuit breakers, and PII redaction directly into existing SDK clients.

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
| `@reaatech/otel-genai-semconv-anthropic` | published v0.1.0 | Instruments the Anthropic Node.js SDK to automatically emit OpenTelemetry spans compliant with GenAI semantic conventions. It provides a class that wraps the Anthropic client to… |
| `@reaatech/otel-genai-semconv-bedrock` | published v0.1.0 | Instruments the AWS Bedrock Runtime SDK to automatically emit OpenTelemetry spans compliant with GenAI semantic conventions. It provides an `instrument` method that wraps the `B… |
| `@reaatech/otel-genai-semconv-core` | published v0.1.0 | Provides TypeScript types, Zod schemas, and a span-builder utility for implementing OpenTelemetry GenAI semantic conventions. It exports constants for attributes, events, and me… |
| `@reaatech/otel-genai-semconv-exporters` | published v0.1.0 | Provides OpenTelemetry `SpanExporter` classes that filter for `gen_ai.*` spans and transform them into the native formats required by Arize Phoenix, Langfuse, and Google Cloud T… |
| `@reaatech/otel-genai-semconv-instrumentation` | published v0.1.0 | Provides a suite of classes for building LLM observability instrumentation, including utilities for OpenTelemetry tracer management, streaming response aggregation, error classi… |
| `@reaatech/otel-genai-semconv-observability` | published v0.1.0 | Provides pre-configured OpenTelemetry SDK initialization, Pino-based structured logging, and GenAI-specific metrics instrumentation for LLM applications. It exports utility func… |
| `@reaatech/otel-genai-semconv-openai` | published v0.1.0 | Instruments the OpenAI Node.js SDK to automatically emit OpenTelemetry spans compliant with GenAI semantic conventions. It provides an `OpenAIInstrumentation` class that wraps t… |
| `@reaatech/otel-genai-semconv-utils` | published v0.1.0 | Provides classes for estimating LLM token usage, calculating request costs based on provider-specific pricing, and recursively redacting PII from nested objects. It exposes `Tok… |
| `@reaatech/otel-genai-semconv-vertexai` | published v0.1.0 | Instruments the Google Generative Language (Vertex AI) SDK to automatically emit OpenTelemetry spans following GenAI semantic conventions. It provides an `instrument` function t… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-otel-genai-semconv`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/otel-genai-semconv
- Browse packages: https://reaatech.com/products/observability-cost/otel-genai-semconv/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: ai-infrastructure, anthropic, arize-phoenix, bedrock, cloud-trace, genai, instrumentation, langfuse, llm, observability, openai, opentelemetry, otel, semantic-conventions, spans, tracing, typescript, vertex-ai
