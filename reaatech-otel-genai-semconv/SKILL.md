---
name: reaatech-otel-genai-semconv
description: "These packages provide instrumented wrappers for OpenAI, Anthropic, Vertex AI, and AWS Bedrock that emit OpenTelemetry-compliant spans for LLM interactions. You would adopt them to gain standardized observability into token usage, costs,…"
license: MIT
---

# REAA otel-genai-semconv

These packages provide instrumented wrappers for OpenAI, Anthropic, Vertex AI, and AWS Bedrock that emit OpenTelemetry-compliant spans for LLM interactions. You would adopt them to gain standardized observability into token usage, costs, streaming performance, and error rates across multiple AI providers. The collection is built on a shared instrumentation framework that uses a zero-config wrapper pattern to inject lifecycle hooks, circuit breakers, and PII redaction directly into existing SDK clients.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Observability & Cost** category. 9 packages live under `@reaatech/otel-genai-semconv-anthropic` and siblings.

## Packages

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

## Quick start

```bash
npm install @reaatech/otel-genai-semconv-anthropic @reaatech/otel-genai-semconv-bedrock @reaatech/otel-genai-semconv-core @reaatech/otel-genai-semconv-exporters @reaatech/otel-genai-semconv-instrumentation @reaatech/otel-genai-semconv-observability @reaatech/otel-genai-semconv-openai @reaatech/otel-genai-semconv-utils @reaatech/otel-genai-semconv-vertexai
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-otel-genai-semconv`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/otel-genai-semconv
- Browse packages: https://reaatech.com/products/observability-cost/otel-genai-semconv/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: ai-infrastructure, anthropic, arize-phoenix, bedrock, cloud-trace, genai, instrumentation, langfuse, llm, observability, openai, opentelemetry, otel, semantic-conventions, spans, tracing, typescript, vertex-ai
