---
name: reaatech-rag-eval-pack
description: "These packages provide a modular toolkit for evaluating RAG systems using heuristic scorers, LLM-as-judge, and automated quality gates. They help teams measure retrieval and generation performance while enforcing cost budgets and CI/CD r…"
license: MIT
---

# REAA rag-eval-pack

These packages provide a modular toolkit for evaluating RAG systems using heuristic scorers, LLM-as-judge, and automated quality gates. They help teams measure retrieval and generation performance while enforcing cost budgets and CI/CD regression thresholds. The system is built as a composable suite where an orchestration engine coordinates data loading, metric calculation, and observability across independent, type-safe packages.

## When to use this

Reach for the `rag-eval-pack` family when the task involves evaluating a RAG pipeline — measuring retrieval precision, generation faithfulness, or overall answer quality. These packages are designed for teams that need a typed, composable evaluation suite that runs in CI/CD, enforces cost budgets, and produces regression gates.

Concrete trigger phrases:
- “set up RAG evaluation metrics” or “measure faithfulness and relevance”
- “add a quality gate for RAG outputs” or “fail CI if retrieval precision drops below X”
- “track LLM judge cost per evaluation run” or “enforce a budget for RAG evals”
- “run an evaluation suite with heuristic and LLM-as-judge metrics”

This family solves the problem of turning ad-hoc RAG evaluation scripts into a repeatable, gated pipeline that fits into a TypeScript monorepo. It provides the schema (`core`), the metric calculators (`metrics` and `judge`), the cost tracker (`cost`), the gate engine (`gate`), and an orchestrator (`suite` and `cli`).

## Quick start example

```typescript
import { EvaluationSuite } from '@reaatech/rag-eval-suite';
import { Dataset } from '@reaatech/rag-eval-dataset';
import { loadYaml } from '@reaatech/rag-eval-dataset';

const dataset = Dataset.fromLoadFunction('samples.yaml', loadYaml);
const suite = new EvaluationSuite({
  dataset,
  metrics: { heuristic: ['faithfulness', 'contextPrecision'] },
  judge: { enabled: true, model: 'openai/gpt-4o' },
  costBudget: { maxCostUsd: 1.50 },
  gate: { thresholds: { faithfulness: 0.8, contextPrecision: 0.75 } },
});

const results = await suite.run();
console.log(results.summary);
```

The `EvaluationSuite` orchestrates dataset loading, heuristic metric calculation, optional LLM judge pass, cost tracking, and gate enforcement in one call. Results include per-sample scores, cost breakdown, and a pass/fail flag for CI.

## Don't reach for this when

- **You need a single metric without the full orchestration.** Use a standalone scorer like RAGAS or the `@reaatech/rag-eval-metrics` package directly instead of the suite.
- **You want a hosted evaluation dashboard or experiment tracking.** This family provides no UI; use LangSmith, Arize, or Weights & Biases for trace visualization.
- **Your evaluation logic lives in Python.** This library is TypeScript-only. For Python RAG evals, use `ragas` or the `langchain` evaluation modules.
- **You need to evaluate a non-RAG system (pure chat, summarization, classification).** The metric definitions are RAG-specialized. Use general-purpose LLM judges or `

## Packages

```bash
npm install @reaatech/rag-eval-cli @reaatech/rag-eval-core @reaatech/rag-eval-cost @reaatech/rag-eval-dataset @reaatech/rag-eval-gate @reaatech/rag-eval-judge @reaatech/rag-eval-mcp-server @reaatech/rag-eval-metrics @reaatech/rag-eval-observability @reaatech/rag-eval-suite
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/rag-eval-cli` | published v0.1.0 | Provides a CLI for executing, gating, and comparing RAG evaluation suites, while also acting as a barrel package that re-exports the entire `@reaatech/rag-eval-*` library for pr… |
| `@reaatech/rag-eval-core` | published v0.1.0 | Provides TypeScript types and Zod schemas for defining RAG evaluation suites, including configurations for judges, cost tracking, and quality gates. It serves as a shared schema… |
| `@reaatech/rag-eval-cost` | published v0.1.0 | Tracks per-sample cost and token usage for RAG evaluations, with configurable budget limits, alert thresholds, and built-in pricing for Anthropic, OpenAI, and Google models. Exp… |
| `@reaatech/rag-eval-dataset` | published v0.1.0 | Manages RAG evaluation datasets by providing classes to load, validate, and version-track samples from JSON, JSONL, and YAML files. It relies on Zod for schema enforcement and i… |
| `@reaatech/rag-eval-gate` | published v0.1.0 | Enforces quality standards on RAG evaluation metrics using a `GateEngine` class that validates results against fixed thresholds or historical baselines. It provides CI-friendly… |
| `@reaatech/rag-eval-judge` | published v0.1.0 | Evaluates RAG pipeline outputs using LLM-as-a-judge with support for multi-model consensus, provider fallbacks, and human-label calibration. It provides a `JudgeEngine` class th… |
| `@reaatech/rag-eval-mcp-server` | published v0.1.0 | An MCP server exposing RAG evaluation as a three‑layer API of atomic judge operations, orchestrated suite runs, and CI regression gates, delivered either as a standalone CLI or… |
| `@reaatech/rag-eval-metrics` | published v0.1.0 | Calculates heuristic-based RAG evaluation metrics including faithfulness, relevance, context precision, and context recall without requiring LLM API calls. It provides individua… |
| `@reaatech/rag-eval-observability` | published v0.1.0 | Provides structured logging via Pino and OpenTelemetry instrumentation for tracing and metrics specific to RAG evaluation workflows. It exports a set of wrapper functions for tr… |
| `@reaatech/rag-eval-suite` | published v0.1.0 | Provides an `EvaluationSuite` class that orchestrates RAG evaluation by running heuristic metrics, an optional LLM judge, per-run cost tracking, quality gates, and dataset |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-rag-eval-pack`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/rag-eval-pack
- Browse packages: https://reaatech.com/products/evals-quality/rag-eval-pack/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, answer-relevance, ci-cd, context-precision, context-recall, evaluation-metrics, faithfulness, llm-eval, mlops, rag, rag-evaluation, retrieval-augmented-generation, testing-tools, typescript
