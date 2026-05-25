---
name: reaatech-rag-eval-pack
description: "These packages give you a full RAG evaluation pipeline—heuristic scorers for faithfulness, relevance, context precision, and context recall, plus an LLM-as-judge with multi-provider support, cost tracking with budget enforcement, and CI…"
license: MIT
---

# REAA rag-eval-pack

These packages give you a full RAG evaluation pipeline—heuristic scorers for faithfulness, relevance, context precision, and context recall, plus an LLM-as-judge with multi-provider support, cost tracking with budget enforcement, and CI quality gates that can fail a build. You'd adopt them to catch regressions in a RAG system before deployment, whether that's a pre-commit smoke check or a nightly regression suite. The distinctive design is that every metric can run at three fidelity levels—free lexical scoring, embedding-based semantic scoring, or LLM judging—so you can trade cost for accuracy per use case without changing the evaluation interface.

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
| `@reaatech/rag-eval-cli` | published v0.1.0 | A CLI that runs RAG evaluation suites, quality gates, run comparisons, cost breakdowns, markdown reports, LLM-based judging, and an MCP server, exposed as the `rag-eval-pack` co… |
| `@reaatech/rag-eval-core` | published v0.1.0 | Canonical TypeScript types and Zod schemas for RAG evaluation data shapes. Exports 18+ types (`EvaluationSample`, `EvalSuiteConfig`, `SampleEvalResult`, `GateConfig`, `JudgeConf… |
| `@reaatech/rag-eval-cost` | published v0.1.0 | Cost tracking, pricing, budgeting, and reporting infrastructure for RAG evaluations, providing `CostTracker`, `Pricing`, `BudgetManager`, and `CostReporter` classes that track p… |
| `@reaatech/rag-eval-dataset` | published v0.1.0 | A Zod-validated dataset loader and validator for RAG evaluation samples, supporting JSONL, JSON, and YAML formats with duplicate detection, synthetic generation from templates,… |
| `@reaatech/rag-eval-gate` | published v0.1.0 | A quality gate engine for RAG evaluation pipelines that enforces threshold-based metric checks and baseline regression detection, returning a `GateResult` object with pass/fail… |
| `@reaatech/rag-eval-judge` | published v0.1.0 | A TypeScript class (`JudgeEngine`) that uses an LLM (Anthropic, OpenAI, or Google) to score RAG outputs on metrics like faithfulness and relevance, with optional consensus votin… |
| `@reaatech/rag-eval-mcp-server` | published v0.1.0 | An MCP server that exposes RAG evaluation tools as a three-layer API of atomic judge operations, orchestrated suite runs, and CI-style regression gates, providing `createMcpServ… |
| `@reaatech/rag-eval-metrics` | published v0.1.0 | Provides four heuristic metric scorers (faithfulness, relevance, context precision, context recall) for evaluating RAG outputs, plus a `MetricsEngine` orchestrator that runs the… |
| `@reaatech/rag-eval-observability` | published v0.1.0 | Provides structured JSON logging via Pino, OpenTelemetry tracing, and OpenTelemetry metrics specifically for RAG evaluation pipelines, exporting functions like `createLogger`, `… |
| `@reaatech/rag-eval-suite` | published v0.1.0 | A class (`EvaluationSuite`) that orchestrates RAG evaluation runs by executing heuristic metrics, optional LLM judge scoring, cost tracking, and quality gates against a dataset,… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-rag-eval-pack`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/rag-eval-pack
- Browse packages: https://reaatech.com/products/evals-quality/rag-eval-pack/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, answer-relevance, ci-cd, context-precision, context-recall, evaluation-metrics, faithfulness, llm-eval, mlops, rag, rag-evaluation, retrieval-augmented-generation, testing-tools, typescript
