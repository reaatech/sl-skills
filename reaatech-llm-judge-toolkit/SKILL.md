---
name: reaatech-llm-judge-toolkit
description: "These packages provide a modular framework for evaluating LLM-generated text using other LLMs as judges. You would adopt them to automate quality assessment, detect model bias, and calibrate evaluation scores against human-labeled datase…"
license: MIT
---

# REAA llm-judge-toolkit

These packages provide a modular framework for evaluating LLM-generated text using other LLMs as judges. You would adopt them to automate quality assessment, detect model bias, and calibrate evaluation scores against human-labeled datasets. The collection is built around a unified `JudgmentEngine` that integrates pluggable providers, consensus strategies, and caching backends into a consistent, typed pipeline.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Evals & Quality** category. 10 packages live under `@reaatech/llm-judge-bias` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/llm-judge-bias @reaatech/llm-judge-cache @reaatech/llm-judge-calibration @reaatech/llm-judge-cli @reaatech/llm-judge-consensus @reaatech/llm-judge-engine @reaatech/llm-judge-infra @reaatech/llm-judge-providers @reaatech/llm-judge-templates @reaatech/llm-judge-types
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/llm-judge-bias` | published v0.1.0 | Identifies systematic position, length, and style biases in LLM evaluations using detector classes that analyze response scores against configurable thresholds. It provides a `C… |
| `@reaatech/llm-judge-cache` | published v0.1.0 | Provides a `CacheManager` class to store and retrieve LLM judgment results using deterministic SHA-256 keys. It supports in-memory, file-system, and Redis backends, with the Red… |
| `@reaatech/llm-judge-calibration` | published v0.1.0 | Measures LLM judge accuracy against human-labeled datasets using a `CalibrationRunner` class for batch evaluation and a `CalibrationMetrics` utility for computing Cohen's kappa,… |
| `@reaatech/llm-judge-cli` | published v0.1.0 | Provides a CLI for batch-evaluating LLM responses and calibrating judgment criteria against human-labeled datasets using JSONL input. It supports multiple LLM providers and conf… |
| `@reaatech/llm-judge-consensus` | published v0.1.0 | Aggregates multiple LLM evaluation scores into a single consensus result using strategies like majority voting, weighted voting, or cost-optimized tiebreaking. It provides a set… |
| `@reaatech/llm-judge-engine` | published v0.1.0 | Provides the core engine for evaluating LLM responses against a given query and context, orchestrating provider calls with retry, caching, rate limiting, and a typed event bus.… |
| `@reaatech/llm-judge-infra` | published v0.1.0 | Infrastructure utilities for LLM judgment pipelines: a `CostTracker` with period-aware budget enforcement, a `BatchProcessor` with configurable concurrency and automatic retry,… |
| `@reaatech/llm-judge-providers` | published v0.1.0 | Provides a unified interface and factory for interacting with OpenAI, Anthropic, and local OpenAI-compatible LLM APIs. It includes built-in cost calculation and health checks, l… |
| `@reaatech/llm-judge-templates` | published v0.1.0 | Provides a set of TypeScript classes implementing a `JudgmentTemplate` interface to generate LLM evaluation prompts and parse their structured JSON responses. Each template incl… |
| `@reaatech/llm-judge-types` | published v0.1.0 | Shared TypeScript types, Zod schemas, and error classes used across the LLM Judge Toolkit ecosystem; requires only `zod` at runtime. Exports 70+ types, 6 error classes, and |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-llm-judge-toolkit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/llm-judge-toolkit
- Browse packages: https://reaatech.com/products/evals-quality/llm-judge-toolkit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, developer-tools, generative-ai, llm, openai, typescript
