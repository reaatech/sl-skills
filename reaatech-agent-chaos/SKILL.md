---
name: reaatech-agent-chaos
description: "These packages provide a fault injection toolkit for testing the resilience of LLM agent systems against real-world failures like latency, rate limits, and malformed tool outputs. You would adopt these to validate that your circuit break…"
license: MIT
---

# REAA agent-chaos

These packages provide a fault injection toolkit for testing the resilience of LLM agent systems against real-world failures like latency, rate limits, and malformed tool outputs. You would adopt these to validate that your circuit breakers, fallbacks, and error-handling logic function correctly when external dependencies fail. The system uses a transparent middleware architecture that intercepts tool calls, allowing you to inject faults declaratively via YAML or JSON without modifying your agent's core implementation.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Testing & Security** category. 6 packages live under `@reaatech/agent-chaos-cli` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-chaos-cli` | published v0.1.0 | Provides a CLI and programmatic API for managing, validating, and executing chaos engineering scenarios for AI agents. It supports project initialization, template generation, a… |
| `@reaatech/agent-chaos-core` | published v0.1.0 | A middleware |
| `@reaatech/agent-chaos-scenarios` | published v0.1.0 | Provides a `ScenarioLoader` class and `SchemaValidator` utility to parse, validate, and hot-reload YAML or JSON chaos injection scenarios. It supports scenario composition via i… |
| `@reaatech/agent-chaos-adapters` | pending npm | Framework adapters that wrap agent tools for LangChain, LlamaIndex, Vercel AI SDK, or any custom tool-call interface, transparently injecting faults from |
| `@reaatech/agent-chaos-e2e` | pending npm | End-to-end test suite that validates the full agent-chaos pipeline, including scenario loading, schema validation, fault injection, engine event recording, and CLI execution aga… |
| `@reaatech/agent-chaos-observability` | pending npm | Structured logging, metrics collection, OpenTelemetry tracing, and report generation for agent-chaos fault injection experiments. It provides pluggable collectors and report gen… |

## Quick start

```bash
npm install @reaatech/agent-chaos-cli @reaatech/agent-chaos-core @reaatech/agent-chaos-scenarios
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-chaos`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-chaos
- Browse packages: https://reaatech.com/products/testing-security/agent-chaos/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, chaos-engineering, developer-tools, fault-injection, llm, mcp, reliability, testing, typescript
