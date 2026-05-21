---
name: reaatech-agent-eval-harness
description: "These packages provide a modular toolkit for evaluating AI agent performance, covering trajectory analysis, tool-use validation, cost tracking, and latency monitoring. You would adopt them to implement automated regression testing, CI/CD…"
license: MIT
---

# REAA agent-eval-harness

These packages provide a modular toolkit for evaluating AI agent performance, covering trajectory analysis, tool-use validation, cost tracking, and latency monitoring. You would adopt them to implement automated regression testing, CI/CD quality gates, and LLM-as-judge scoring for your agent workflows. The collection is built as a set of independent, composable TypeScript packages that can be used via a CLI, integrated into test suites, or exposed through an MCP server for AI-assisted development.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Evals & Quality** category. 13 packages live under `@reaatech/agent-eval-harness-cli` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-eval-harness-cli` | published v0.1.0 | This CLI provides a suite of commands for executing agent evaluation pipelines, managing golden trajectories, and enforcing CI quality gates. It also functions as an MCP server… |
| `@reaatech/agent-eval-harness-cost` | published v0.1.0 | Calculates and enforces spending limits for AI agent trajectories by providing functions to compute token-based costs, compare performance, and trigger budget alerts. It exports… |
| `@reaatech/agent-eval-harness-gate` | published v0.1.0 | Enforces CI/CD regression thresholds for AI agent performance, cost, and quality metrics. It provides a `GateEngine` class to evaluate agent results against configurable gates a… |
| `@reaatech/agent-eval-harness-golden` | published v0.1.0 | Manages reference agent trajectories for regression testing through a collection of utility functions and a `GoldenCurator` class. It provides tools to create, annotate, and val… |
| `@reaatech/agent-eval-harness-judge` | published v0.1.0 | Evaluates agent responses using LLM-as-a-judge patterns with support for multi-model consensus, automated calibration, and cost tracking. It provides a `JudgeEngine` class that… |
| `@reaatech/agent-eval-harness-latency` | published v0.1.0 | Computes latency metrics, enforces SLA budgets, and identifies performance bottlenecks for AI agent trajectories. It provides a suite of utility functions and a `LatencyTracker`… |
| `@reaatech/agent-eval-harness-mcp-server` | published v0.1.0 | Exposes 13 evaluation tools for AI agents via the Model Context Protocol (MCP) using stdio transport. It provides a factory function to instantiate a server that handles atomic… |
| `@reaatech/agent-eval-harness-observability` | published v0.1.0 | Provides OpenTelemetry instrumentation, Pino-based structured logging with PII redaction, and an in-memory dashboard manager for tracking agent evaluation pipelines. It exposes… |
| `@reaatech/agent-eval-harness-suite` | published v0.1.0 | Executes batch evaluations of agent trajectories using a YAML-configured runner class that aggregates multi-metric scores and performs statistical regression analysis between ru… |
| `@reaatech/agent-eval-harness-tool-use` | published v0.1.0 | Validates agent tool-use trajectories by checking schema compliance, argument accuracy, and result integration. It provides a set of utility functions to evaluate individual too… |
| `@reaatech/agent-eval-harness-trajectory` | published v0.1.0 | Provides utilities for loading, validating, and evaluating agent conversation trajectories from JSONL files. It exports functions for parsing data, calculating coherence and goa… |
| `@reaatech/agent-eval-harness-types` | published v0.1.0 | Provides TypeScript interfaces and Zod schemas for defining agent trajectories, evaluation results, latency budgets, and regression gates. It serves as a shared type library for… |
| `@reaatech/agent-eval-harness-infra` | pending npm | Provides a collection of Terraform modules and environment configurations for deploying the agent-eval-harness across AWS, Azure, GCP, OCI, Vercel, and Netlify. It requires Terr… |

## Quick start

```bash
npm install @reaatech/agent-eval-harness-cli @reaatech/agent-eval-harness-cost @reaatech/agent-eval-harness-gate @reaatech/agent-eval-harness-golden @reaatech/agent-eval-harness-judge @reaatech/agent-eval-harness-latency @reaatech/agent-eval-harness-mcp-server @reaatech/agent-eval-harness-observability @reaatech/agent-eval-harness-suite @reaatech/agent-eval-harness-tool-use @reaatech/agent-eval-harness-trajectory @reaatech/agent-eval-harness-types
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-eval-harness`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-eval-harness
- Browse packages: https://reaatech.com/products/evals-quality/agent-eval-harness/packages
- npm scope: https://www.npmjs.com/~reaatech
