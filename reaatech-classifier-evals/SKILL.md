---
name: reaatech-classifier-evals
description: "These packages give you a complete offline evaluation harness for intent classification systems, covering dataset loading, metrics calculation, LLM-as-judge evaluation, regression quality gates, and result export. You would adopt them to…"
license: MIT
---

# REAA classifier-evals

These packages give you a complete offline evaluation harness for intent classification systems, covering dataset loading, metrics calculation, LLM-as-judge evaluation, regression quality gates, and result export. You would adopt them to run rigorous, repeatable evaluations of classifier models in CI pipelines and production workflows, catching regressions before they ship. The most distinctive thing is that every component—from Zod-validated schemas to OpenTelemetry spans to MCP server tools—shares a single set of canonical types, so you can compose dataset loaders, metric calculators, judge engines, and gate checkers into a single pipeline without adapter code.

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
| `@reaatech/classifier-evals` | published v0.1.1 | Canonical TypeScript types, Zod schemas, and shared utilities (structured logging, OpenTelemetry tracing/metrics, PII redaction, hashing) for the classifier-evals evaluation eco… |
| `@reaatech/classifier-evals-cli` | published v0.1.1 | A CLI for running classifier evaluations, comparing models, checking regression gates, and exporting results, built on Commander.js and the `@reaatech/classifier-evals-*` ecosys… |
| `@reaatech/classifier-evals-dataset` | published v0.1.1 | A dataset loading and validation utility for classifier evaluation, supporting CSV, JSON, and JSONL formats. Provides functions (`loadDataset`, `validateDataset`, `splitDataset`… |
| `@reaatech/classifier-evals-exporters` | published v0.1.1 | Export classifier evaluation results as JSON, HTML, Arize Phoenix traces, or Langfuse traces. Provides four functions (`exportToJson`, `exportToHtml`, `exportToPhoenix`, `export… |
| `@reaatech/classifier-evals-gates` | published v0.1.1 | A gate evaluation engine that checks classifier metrics (accuracy, F1, precision, recall) against threshold, baseline-comparison, and distribution gates, returning pass/fail res… |
| `@reaatech/classifier-evals-judge` | published v0.1.1 | A function that creates an LLM-as-judge engine for evaluating classifier outputs, supporting Anthropic and OpenAI models with configurable consensus voting, real-time cost track… |
| `@reaatech/classifier-evals-mcp-server` | published v0.1.1 | An MCP server that exposes five tools (`run_eval`, `check_gates`, `compare_models`, `llm_judge`, `generate_report`) for running classifier evaluation pipelines, checking regress… |
| `@reaatech/classifier-evals-metrics` | published v0.1.1 | A function that computes confusion matrices, 14 classification metrics (accuracy, macro/micro/weighted precision/recall/F1, MCC, Cohen's Kappa), model comparison with McNemar's… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-classifier-evals`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/classifier-evals
- Browse packages: https://reaatech.com/products/evals-quality/classifier-evals/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, arize-phoenix, ci-cd, classifier, confusion-matrix, evaluation-harness, intent-classification, langfuse, llm-as-judge, llm-eval, mlops, observability, regression-testing, testing-tools, typescript
