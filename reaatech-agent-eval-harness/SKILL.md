---
name: reaatech-agent-eval-harness
description: "These packages give you a full evaluation pipeline for AI agent trajectories—loading multi-turn conversations, scoring them on quality, tool correctness, cost, and latency, then running those scores through CI/CD regression gates. You'd…"
license: MIT
---

# REAA agent-eval-harness

These packages give you a full evaluation pipeline for AI agent trajectories—loading multi-turn conversations, scoring them on quality, tool correctness, cost, and latency, then running those scores through CI/CD regression gates. You'd adopt them to catch regressions in agent behavior before deploying, replacing ad-hoc manual review or single-metric checks with structured, repeatable evaluation. The monorepo is organized as independent packages (trajectory loading, tool-use validation, cost tracking, LLM-as-judge, golden comparisons, suite orchestration, CI gates, MCP server, observability) that each export plain TypeScript functions and Zod schemas, so you compose exactly the pieces you need without a framework lock-in.

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
| `@reaatech/agent-eval-harness-cli` | published v0.1.1 | A CLI providing 7 subcommands (`eval`, `judge`, `compare`, `gate`, `golden`, `report`, `serve`) for running and managing LLM agent evaluations, including trajectory loading, mul… |
| `@reaatech/agent-eval-harness-cost` | published v0.1.0 | Calculates per-task LLM token and tool invocation costs for AI agent trajectories, with budget enforcement and cost reporting. Exports functions like `calculateTrajectoryCost`,… |
| `@reaatech/agent-eval-harness-gate` | published v0.1.1 | A function that evaluates AI agent evaluation results against configurable quality, cost, latency, and correctness thresholds, returning a pass/fail summary for CI/CD gating. |
| `@reaatech/agent-eval-harness-golden` | published v0.1.0 | A library for creating, managing, and comparing golden reference trajectories against candidate agent runs to detect regressions. It provides functions (`createGolden`, `compare… |
| `@reaatech/agent-eval-harness-judge` | published v0.1.0 | A provider-agnostic LLM-as-judge engine that scores agent responses on faithfulness, relevance, tool correctness, and overall quality using Claude, GPT-4, Gemini, or any OpenAI-… |
| `@reaatech/agent-eval-harness-latency` | published v0.1.0 | A latency monitoring and SLA enforcement toolkit for AI agent evaluation, providing functions to compute P50/P90/P99 percentiles per turn and trajectory, detect anomalous turns,… |
| `@reaatech/agent-eval-harness-mcp-server` | published v0.1.1 | An MCP (Model Context Protocol) server that exposes 13 evaluation tools across three layers—atomic judge operations, orchestrated suite runs, and CI gate operations—via stdio tr… |
| `@reaatech/agent-eval-harness-observability` | published v0.1.1 | Provides OpenTelemetry tracing, 7 pre-configured metrics, Pino-based structured logging with automatic PII redaction, and an in-memory dashboard manager with trend analysis and… |
| `@reaatech/agent-eval-harness-suite` | published v0.1.1 | A YAML-driven batch evaluation runner that executes multi-metric assessments across trajectory collections with configurable concurrency, then aggregates results into JSON, JUni… |
| `@reaatech/agent-eval-harness-tool-use` | published v0.1.0 | Validates tool selection, argument schema compliance, result hallucination, and integration for agent tool calls across trajectories, exporting functions like `validateToolCall`… |
| `@reaatech/agent-eval-harness-trajectory` | published v0.1.0 | Parses JSONL turn files into validated trajectories, then scores them for coherence, goal completion, and conversation flow, and diffs candidate trajectories against golden refe… |
| `@reaatech/agent-eval-harness-types` | published v0.1.0 | Canonical TypeScript domain types, Zod schemas, and interfaces for the agent-eval-harness ecosystem. Exports 19 interfaces (`Turn`, `Trajectory`, `EvalResult`, `JudgeScore`, `Co… |
| `@reaatech/agent-eval-harness-infra` | pending npm | Terraform modules and environment configurations for deploying the agent-eval-harness application to AWS, Azure, GCP, OCI, Netlify, and Vercel, providing reusable infrastructure… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-eval-harness`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-eval-harness
- Browse packages: https://reaatech.com/products/evals-quality/agent-eval-harness/packages
- npm scope: https://www.npmjs.com/~reaatech
