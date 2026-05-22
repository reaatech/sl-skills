---
name: reaatech-agent-eval-harness
description: "These packages provide a modular toolkit for evaluating AI agent performance, covering trajectory analysis, tool-use validation, cost tracking, and latency monitoring. You would adopt them to implement automated regression testing, CI/CD…"
license: MIT
---

# REAA agent-eval-harness

These packages provide a modular toolkit for evaluating AI agent performance, covering trajectory analysis, tool-use validation, cost tracking, and latency monitoring. You would adopt them to implement automated regression testing, CI/CD quality gates, and LLM-as-judge scoring for your agent workflows. The collection is built as a set of independent, composable TypeScript packages that can be used via a CLI, integrated into test suites, or exposed through an MCP server for AI-assisted development.

## When to use this

Reach for the **agent-eval-harness** family when you need to automate quality checks on AI agent behavior — not just log outputs, but structurally validate tool calls, measure cost and latency, and compare trajectories against golden references. These packages are designed to embed evaluation into CI/CD pipelines: gate deployments on regression thresholds, generate JUnit XML for test reporting, or expose evaluation tools via MCP for agent-assisted development.

Concrete trigger phrases that signal this family is appropriate:  
- "We want to run regression tests on our agent every time we deploy."  
- "Our agent’s tool calls are drifting — can we flag schema mismatches automatically?"  
- "We need a pass/fail gate that blocks PRs if latency or cost exceeds a budget."  
- "Can we use an LLM-as-a-judge to score response quality and compare versions?"

This category solves the “how do I know my agent is still good after a prompt update or model change?” problem. It’s meant for teams who ship agent workflows and need structured, repeatable evaluation — not ad-hoc manual scoring.

## Quick start example

The following snippet loads a trajectory from a JSONL file, runs a LatencyTracker to check SLA, and then enforces a CI gate that fails if the total cost exceeds $0.10.

```typescript
import { loadTrajectory } from '@reaatech/agent-eval-harness-trajectory';
import { LatencyTracker } from '@reaatech/agent-eval-harness-latency';
import { GateEngine } from '@reaatech/agent-eval-harness-gate';

const trajectory = await loadTrajectory('./trajectories/session-01.jsonl');

const latencyTracker = new LatencyTracker({ slaMs: 5000 });
const latencyResult = latencyTracker.analyze(trajectory);

const gate = new GateEngine({
  rules: [
    { metric: 'cost', max: 0.10 },
    { metric: 'latency.p95', max: 5000 },
  ],
});

const gateResult = await gate.evaluate(trajectory);
if (!gateResult.passed) {
  process.exit(1); // Block CI
}
```

## Don’t reach for this when

- **You need real-time monitoring of a live agent in production.**  
  Use `@reaatech/agent-observability` (telemetry and dashboards) instead — this family is designed for offline evaluation and CI gates, not live span streaming.

- **You want to tune prompts or model parameters automatically.**  
  This is an evaluation toolkit, not an optimizer. For automated prompt iteration, pairing, or RL-based tuning, look at a dedicated hyperparameter optimization library or `autogen`-style frameworks.

- **Your evaluation is purely qualitative, no numeric metrics (e.g., “does the agent sound friendly?”).**  
  The `@reaatech/agent-eval-harness-judge` package can do LLM-as-a-judge for subjectivity, but if you need human-rating workflows, custom rubrics, or A/B preference collection, consider a purpose-built eval platform like LangSmith or a simple survey tool.

- **You are evaluating a rule‑based system with no AI models.**  
  This family expects agent trajectories with LLM interaction patterns (tool calls, turns, costs). For deterministic rule validation, a standard test framework like Vitest or Jest is simpler and faster.

- **You need to deploy the evaluation infrastructure on a cloud provider.**  
  The `@reaatech/agent-eval-harness-infra` package (Terraform modules) is still pending — use manual setup or your existing cloud deployment tooling for now.

## Packages

```bash
npm install @reaatech/agent-eval-harness-cli @reaatech/agent-eval-harness-cost @reaatech/agent-eval-harness-gate @reaatech/agent-eval-harness-golden @reaatech/agent-eval-harness-judge @reaatech/agent-eval-harness-latency @reaatech/agent-eval-harness-mcp-server @reaatech/agent-eval-harness-observability @reaatech/agent-eval-harness-suite @reaatech/agent-eval-harness-tool-use @reaatech/agent-eval-harness-trajectory @reaatech/agent-eval-harness-types
```

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

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-eval-harness`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-eval-harness
- Browse packages: https://reaatech.com/products/evals-quality/agent-eval-harness/packages
- npm scope: https://www.npmjs.com/~reaatech
