---
name: reaatech-rag-eval-pack
description: "These packages provide a modular toolkit for evaluating RAG systems using heuristic scorers, LLM-as-judge, and automated quality gates. They help teams measure retrieval and generation performance while enforcing cost budgets and CI/CD r…"
license: MIT
---

# REAA rag-eval-pack

These packages provide a modular toolkit for evaluating RAG systems using heuristic scorers, LLM-as-judge, and automated quality gates. They help teams measure retrieval and generation performance while enforcing cost budgets and CI/CD regression thresholds. The system is built as a composable suite where an orchestration engine coordinates data loading, metric calculation, and observability across independent, type-safe packages.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Evals & Quality** category. 10 packages live under `@reaatech/rag-eval-cli` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/rag-eval-cli` | pending npm | Provides a CLI for executing, gating, and comparing RAG evaluation suites, while also acting as a barrel package that re-exports the entire `@reaatech/rag-eval-*` library for pr… |
| `@reaatech/rag-eval-core` | pending npm | Provides TypeScript types and Zod schemas for defining RAG evaluation suites, including configurations for judges, cost tracking, and quality gates. It serves as a shared schema… |
| `@reaatech/rag-eval-cost` | pending npm | Tracks per-sample cost and token usage for RAG evaluations, with configurable budget limits, alert thresholds, and built-in pricing for Anthropic, OpenAI, and Google models. Exp… |
| `@reaatech/rag-eval-dataset` | pending npm | Manages RAG evaluation datasets by providing classes to load, validate, and version-track samples from JSON, JSONL, and YAML files. It relies on Zod for schema enforcement and i… |
| `@reaatech/rag-eval-gate` | pending npm | Enforces quality standards on RAG evaluation metrics using a `GateEngine` class that validates results against fixed thresholds or historical baselines. It provides CI-friendly… |
| `@reaatech/rag-eval-judge` | pending npm | Evaluates RAG pipeline outputs using LLM-as-a-judge with support for multi-model consensus, provider fallbacks, and human-label calibration. It provides a `JudgeEngine` class th… |
| `@reaatech/rag-eval-mcp-server` | pending npm | An MCP server exposing RAG evaluation as a three‑layer API of atomic judge operations, orchestrated suite runs, and CI regression gates, delivered either as a standalone CLI or… |
| `@reaatech/rag-eval-metrics` | pending npm | Calculates heuristic-based RAG evaluation metrics including faithfulness, relevance, context precision, and context recall without requiring LLM API calls. It provides individua… |
| `@reaatech/rag-eval-observability` | pending npm | Provides structured logging via Pino and OpenTelemetry instrumentation for tracing and metrics specific to RAG evaluation workflows. It exports a set of wrapper functions for tr… |
| `@reaatech/rag-eval-suite` | pending npm | Provides an `EvaluationSuite` class that orchestrates RAG evaluation by running heuristic metrics, an optional LLM judge, per-run cost tracking, quality gates, and dataset |

## Quick start

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-rag-eval-pack`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/rag-eval-pack
- Browse packages: https://reaatech.com/products/evals-quality/rag-eval-pack/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, answer-relevance, ci-cd, context-precision, context-recall, evaluation-metrics, faithfulness, llm-eval, mlops, rag, rag-evaluation, retrieval-augmented-generation, testing-tools, typescript
