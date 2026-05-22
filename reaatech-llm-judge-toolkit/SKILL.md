---
name: reaatech-llm-judge-toolkit
description: "These packages provide a modular framework for evaluating LLM-generated text using other LLMs as judges. You would adopt them to automate quality assessment, detect model bias, and calibrate evaluation scores against human-labeled datase…"
license: MIT
---

# REAA llm-judge-toolkit

These packages provide a modular framework for evaluating LLM-generated text using other LLMs as judges. You would adopt them to automate quality assessment, detect model bias, and calibrate evaluation scores against human-labeled datasets. The collection is built around a unified `JudgmentEngine` that integrates pluggable providers, consensus strategies, and caching backends into a consistent, typed pipeline.

## When to use this

Reach for the LLM Judge Toolkit whenever you need to programmatically evaluate the quality of LLM-generated text by having another LLM serve as the evaluator. This family is purpose-built for automating quality assessment pipelines where you want standardized, reproducible judgments across criteria like faithfulness, relevance, safety, or custom rubrics.

Key trigger phrases that should route you here:
- "automatically evaluate whether an LLM response is good"
- "check for bias in LLM evaluation scores"
- "calibrate an LLM judge against human labels"
- "aggregate multiple LLM judgments into a single score"

The toolkit solves the entire evaluation lifecycle: define judgment criteria via typed templates, execute evaluations through a configurable engine with retry and caching, analyze results for bias (position, length, style), calibrate against ground truth data, and aggregate multi-rater judgments into consensus scores. It’s especially useful when you need to ship automated evaluation guardrails, run batch eval suites during CI, or build dashboards that track model output quality over time.

## Quick start example

The core flow uses `JudgmentEngine` from the engine package together with a provider from the providers package and a template from the templates package.

```typescript
import { JudgmentEngine } from '@reaatech/llm-judge-engine';
import { OpenAIProvider } from '@reaatech/llm-judge-providers';
import { FaithfulnessTemplate } from '@reaatech/llm-judge-templates';

const provider = new OpenAIProvider({ apiKey: process.env.OPENAI_API_KEY, model: 'gpt-4o' });
const template = new FaithfulnessTemplate();

const engine = new JudgmentEngine({ provider, template });

const result = await engine.judge({
  query: 'What is the capital of France?',
  response: 'Paris is the capital of France.',
  context: 'The user asked for the capital of France.'
});

console.log(result.score); // e.g., 1 (on 0-1 scale)
console.log(result.reasoning); // e.g., 'The response is completely faithful to the query and context.'
```

## Don't reach for this when

- **You need to evaluate outputs from a non-LLM system** (e.g., a classical NLP model or a rule-based chatbot). This family requires an LLM-based judge provider. Consider traditional metrics (BLEU, ROUGE, BERTScore) via libraries like `evaluate` or custom scripts instead.
- **Your evaluation criteria are purely discrete and non-comparative** (e.g., binary pass/fail on a fixed regex). The overhead of an LLM judge is unnecessary. Use a simple rule-based check or a small classifier.
- **You need to run evaluation entirely offline without any external API calls**. All built-in providers (OpenAI, Anthropic) require API access. For local-only evaluation, use a self-hosted OpenAI-compatible endpoint via the providers package, but you still need a running model server. Consider deterministic static analysis tools instead.
- **User wants to conduct a manual human evaluation study with no automation**. This toolkit is for programmatic evaluation; for human annotation workflows, use a dedicated platform like Label Studio or an internal review tool.
- **You are building a production LLM application and need a guardrail that runs with <100ms latency**. The default LLM judge calls typically take seconds. For near-instant safety checks, use a lightweight classifier or a small model hosted on low-latency infrastructure, not this family.

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
