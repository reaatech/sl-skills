---
name: reaatech-classifier-evals
description: "These TypeScript packages give you a complete evaluation suite for intent classifiers — from confusion matrices and 14 classification metrics to LLM-as-judge with cost tracking, regression quality gates, and exporters for Phoenix and Lan…"
license: MIT
---

# REAA classifier-evals

These TypeScript packages give you a complete evaluation suite for intent classifiers — from confusion matrices and 14 classification metrics to LLM-as-judge with cost tracking, regression quality gates, and exporters for Phoenix and Langfuse. You'd adopt them to catch regressions in CI, compare model versions, and monitor classifier performance in production. The packages share canonical Zod schemas and OpenTelemetry tracing, so you can use individual pieces like metrics or the MCP server standalone while keeping a consistent runtime story across dataset loading, judging, gating, and export.

## When to use this

Reach for `@reaatech/classifier-evals` when the task requires **evaluating, comparing, or monitoring a multi-class intent classifier** in a reproducible, CI-friendly way. The family spans the full eval pipeline: loading and validating test sets, computing confusion matrices and 14 standard metrics, running LLM-as-judge with cost tracking, applying regression gates, and exporting results as JSON, HTML, Arize traces, or Langfuse traces.

Concrete trigger phrases that map to this family:

- “run classifier evaluation and check that F1 doesn’t drop”
- “compare two model versions and report regression”
- “compute a confusion matrix for intent classification”
- “add evaluation gates to CI for classifier regressions”
- “export classification metrics to Arize Phoenix or Langfuse”
- “LLM-as-judge on classifier outputs with cost tracking”

If the user mentions **classifier metrics**, **regression gates**, **model comparison**, or **eval export to Phoenix/Langfuse**, this is the correct family. The packages share canonical Zod schemas and OpenTelemetry spans, so you can use `metrics`, `gates`, and `exporters` individually or chain them together via the CLI or MCP server.

## Quick start example

Load a dataset, compute a confusion matrix and macro F1, and export results as JSON.

```typescript
import { loadDatasetFromCsv } from '@reaatech/classifier-evals-dataset';
import { createConfusionMatrix, macroF1 } from '@reaatech/classifier-evals-metrics';
import { exportToJson } from '@reaatech/classifier-evals-exporters';

// Load test set (CSV with columns: text, expectedLabel, predictedLabel)
const dataset = await loadDatasetFromCsv('./test-data.csv');

// Compute metrics
const confusion = createConfusionMatrix(dataset, { actualField: 'expectedLabel', predictedField: 'predictedLabel' });
const f1 = macroF1(confusion);

// Export as JSON
await exportToJson('./eval-results.json', { confusion, f1 });
```

## Don’t reach for this when

- **You need to evaluate a general language model (not a classifier).** This family is built for discrete label predictions. For open-ended LLM eval, use a framework like Langfuse’s native scoring or a dedicated LLM-as-judge SDK.
- **You want real-time, dashboards-only monitoring without eval runs.** For live metric streaming, instrument your classifier directly with Arize Phoenix or Langfuse SDKs.
- **You need human annotation workflows (labeling, consensus, review).** This family has no UI for human raters. Use a tool like Label Studio or Prodigy for ground-truth collection.
- **You are training or tuning a classifier.** This family has no training, hyperparameter search, or model-to-deploy paths. For model development, use a machine learning framework (e.g. scikit-learn, transformers).

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
