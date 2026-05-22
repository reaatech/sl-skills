---
name: reaatech-classifier-evals
description: "These TypeScript packages give you a complete evaluation suite for intent classifiers — from confusion matrices and 14 classification metrics to LLM-as-judge with cost tracking, regression quality gates, and exporters for Phoenix and Lan…"
license: MIT
---

# REAA classifier-evals

These TypeScript packages give you a complete evaluation suite for intent classifiers — from confusion matrices and 14 classification metrics to LLM-as-judge with cost tracking, regression quality gates, and exporters for Phoenix and Langfuse. You'd adopt them to catch regressions in CI, compare model versions, and monitor classifier performance in production. The packages share canonical Zod schemas and OpenTelemetry tracing, so you can use individual pieces like metrics or the MCP server standalone while keeping a consistent runtime story across dataset loading, judging, gating, and export.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Evals & Quality** category. 8 packages live under `@reaatech/classifier-evals` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/classifier-evals @reaatech/classifier-evals-cli @reaatech/classifier-evals-dataset @reaatech/classifier-evals-exporters @reaatech/classifier-evals-gates @reaatech/classifier-evals-judge @reaatech/classifier-evals-mcp-server @reaatech/classifier-evals-metrics
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/classifier-evals` | published v0.1.0 | TypeScript types, Zod schemas, and utility functions (Pino logger with PII redaction, OpenTelemetry instrumentation, SHA |
| `@reaatech/classifier-evals-cli` | published v0.1.0 | A CLI built on Commander.js for running classifier evaluations, comparing model results, checking regression gates, and exporting reports, all from the command line using the @r… |
| `@reaatech/classifier-evals-dataset` | published v0.1.0 | Provides utilities for loading, validating, and partitioning classifier evaluation datasets from CSV, JSON, or JSONL files. It exports a set of functions for performing stratifi… |
| `@reaatech/classifier-evals-exporters` | published v0.1.0 | Provides functions (`exportToJson`, `exportToHtml`, `exportToPhoenix`, `exportToLangfuse`) to serialize classifier evaluation results into JSON, an interactive HTML report with… |
| `@reaatech/classifier-evals-gates` | published v0.1.0 | Evaluates threshold, baseline-comparison, and distribution quality gates against classification metrics, returning a `GateEngine` that runs gates and formats results as GitHub A… |
| `@reaatech/classifier-evals-judge` | published v0.1.0 | Evaluates classification model outputs using LLM-as-a-judge with support for consensus voting, real-time cost tracking, and PII redaction. It provides a `createJudgeEngine` fact… |
| `@reaatech/classifier-evals-mcp-server` | published v0.1.0 | Exposes classifier evaluation workflows—including running evaluations, checking regression gates, and performing LLM-as-judge comparisons—as a set of Model Context Protocol (MCP… |
| `@reaatech/classifier-evals-metrics` | published v0.1.0 | Calculates classification performance metrics, including confusion matrices, multi-class F1 scores, and statistical model comparisons. It provides a collection of utility functi… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-classifier-evals`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/classifier-evals
- Browse packages: https://reaatech.com/products/evals-quality/classifier-evals/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, arize-phoenix, ci-cd, classifier, confusion-matrix, evaluation-harness, intent-classification, langfuse, llm-as-judge, llm-eval, mlops, observability, regression-testing, testing-tools, typescript
