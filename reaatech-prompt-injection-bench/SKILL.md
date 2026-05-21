---
name: reaatech-prompt-injection-bench
description: "These packages provide a standardized framework for benchmarking and evaluating the effectiveness of prompt-injection defenses in AI agent systems. They allow you to measure security posture by running a diverse corpus of adversarial att…"
license: MIT
---

# REAA prompt-injection-bench

These packages provide a standardized framework for benchmarking and evaluating the effectiveness of prompt-injection defenses in AI agent systems. They allow you to measure security posture by running a diverse corpus of adversarial attacks against pluggable defense adapters and calculating statistically significant performance scores. The system is designed as a modular pipeline where you can swap defense implementations, execute parallelized benchmarks, and generate reproducible reports through a unified CLI or MCP-compatible interface.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Testing & Security** category. 9 packages live under `@reaatech/pi-bench-adapters` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/pi-bench-adapters` | published v1.0.1 | Provides a collection of standardized adapter classes and a registry for integrating various prompt injection detection services and libraries. Each adapter implements a common… |
| `@reaatech/pi-bench-core` | published v1.0.1 | Provides TypeScript types, Zod schemas, and a standardized attack taxonomy for validating and scoring prompt injection benchmarks. It exports utility functions and schema object… |
| `@reaatech/pi-bench-corpus` | published v1.0.1 | Generates and validates datasets of prompt injection attacks using a template-based engine that applies obfuscation strategies like synonym replacement and character manipulatio… |
| `@reaatech/pi-bench-leaderboard` | published v1.0.1 | Manages and persists ranked leaderboard data for prompt injection defenses using a factory-provided manager object. It calculates composite scores and assigns performance tiers,… |
| `@reaatech/pi-bench-mcp-server` | published v1.0.1 | Exposes prompt-injection-bench operations as an MCP server, providing tools to execute benchmarks, compare defense results, and generate reports via stdio. It also includes util… |
| `@reaatech/pi-bench-observability` | published v1.0.1 | Provides pre-configured observability utilities for prompt injection benchmarks, including a Pino-based structured logger with PII sanitization, an OpenTelemetry-compatible metr… |
| `@reaatech/pi-bench-runner` | published v1.0.1 | Executes prompt injection benchmarks by running attack suites against defense adapters in parallel with configurable timeouts and progress tracking. It provides factory function… |
| `@reaatech/pi-bench-scoring` | published v1.0.1 | Calculates weighted scores, statistical significance, and effect sizes for prompt injection defense benchmarks. It provides a collection of utility functions for computing metri… |
| `prompt-injection-bench` | published v1.0.1 | A CLI and library for running standardized benchmarks of prompt-injection defense mechanisms, providing attack corpora, defense adapters, and commands to execute, compare, and r… |

## Quick start

```bash
npm install @reaatech/pi-bench-adapters @reaatech/pi-bench-core @reaatech/pi-bench-corpus @reaatech/pi-bench-leaderboard @reaatech/pi-bench-mcp-server @reaatech/pi-bench-observability @reaatech/pi-bench-runner @reaatech/pi-bench-scoring prompt-injection-bench
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-prompt-injection-bench`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/prompt-injection-bench
- Browse packages: https://reaatech.com/products/testing-security/prompt-injection-bench/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: adversarial-testing, agentic-ai, ai-safety, ai-security, benchmark, defense, llm, llm-security, prompt-injection, red-teaming, sanitization, security, test-corpus, typescript
