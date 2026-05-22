---
name: reaatech-guardrail-chain
description: "These packages provide a framework for building safety pipelines that validate and filter LLM inputs and outputs. You would use them to manage complex security requirements, such as PII redaction or prompt injection detection, while stri…"
license: MIT
---

# REAA guardrail-chain

These packages provide a framework for building safety pipelines that validate and filter LLM inputs and outputs. You would use them to manage complex security requirements, such as PII redaction or prompt injection detection, while strictly enforcing latency and token budgets per request. The system uses a fluent `ChainBuilder` to compose guardrails into a single execution pipeline, allowing for short-circuit logic and dynamic scheduling based on remaining budget.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Testing & Security** category. 4 packages live under `@reaatech/guardrail-chain` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/guardrail-chain @reaatech/guardrail-chain-config @reaatech/guardrail-chain-guardrails @reaatech/guardrail-chain-observability
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/guardrail-chain` | published v0.1.0 | Provides a framework for orchestrating sequences of LLM guardrails with built-in budget management, circuit breaking, and retry logic. It exports the `GuardrailChain` orchestrat… |
| `@reaatech/guardrail-chain-config` | published v0.1.0 | Loads and validates Guardrail Chain configurations by merging JSON or YAML files with environment variable overrides. It provides utility functions that return validated configu… |
| `@reaatech/guardrail-chain-guardrails` | published v0.1.0 | Provides thirteen built-in guardrail implementations—covering input validation, output filtering, and caching—as classes that implement the `Guardrail<TInput, TOutput>` interfac… |
| `@reaatech/guardrail-chain-observability` | published v0.1.0 | Provides pluggable interfaces for structured logging, metrics collection, and distributed tracing within the Guardrail Chain framework. It exposes global getter and setter funct… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-guardrail-chain`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/guardrail-chain
- Browse packages: https://reaatech.com/products/testing-security/guardrail-chain/packages
- npm scope: https://www.npmjs.com/~reaatech
