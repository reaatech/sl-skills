---
name: reaatech-llm-judge-toolkit
description: "These packages give you a complete system for using LLMs to evaluate generated text, with built-in prompt templates for five criteria (faithfulness, relevance, coherence, safety, tool-use), multi-provider support (OpenAI, Anthropic, loca…"
license: MIT
---

# REAA llm-judge-toolkit

These packages give you a complete system for using LLMs to evaluate generated text, with built-in prompt templates for five criteria (faithfulness, relevance, coherence, safety, tool-use), multi-provider support (OpenAI, Anthropic, local endpoints), and a judgment engine that handles retries, caching, and rate limiting. You would adopt them to replace ad-hoc LLM evaluation scripts with a structured pipeline that includes statistical calibration against human labels, bias detection (position, length, style), and multi-judge consensus strategies. The packages are designed as independently installable modules that share a common type system and pluggable interfaces—the engine, providers, templates, cache backends, and bias detectors each implement a shared contract, so you can swap implementations or use only the pieces you need without pulling in the rest.

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
| `@reaatech/llm-judge-bias` | published v0.1.0 | A set of classes (`PositionBiasDetector`, `LengthBiasDetector`, `StyleBiasDetector`, and `ComprehensiveBiasDetector`) that detect systematic biases—position, length, and style—i… |
| `@reaatech/llm-judge-cache` | published v0.1.0 | A CacheManager facade with pluggable in-memory, file-system, and Redis backends for caching LLM judgment results, using SHA-256 content-addressed keys and configurable TTL. |
| `@reaatech/llm-judge-calibration` | published v0.1.0 | A TypeScript library that measures how accurately an LLM judge system classifies text against human-labeled gold-standard datasets, providing Cohen's kappa, confusion matrices,… |
| `@reaatech/llm-judge-cli` | published v0.1.0 | A CLI that evaluates LLM responses against criteria (faithfulness, relevance, coherence, safety, tool-use) and calibrates judgments against human labels, reading JSONL input and… |
| `@reaatech/llm-judge-consensus` | published v0.1.0 | Provides consensus strategies (majority voting, weighted voting, and a cheap-first tiebreaker) as classes implementing a shared `ConsensusStrategy` interface, combining individu… |
| `@reaatech/llm-judge-engine` | published v0.1.0 | Orchestrates LLM-based judgment calls with automatic retry, caching, rate limiting, and a typed event bus, exposing a `JudgmentEngine` class that takes a provider, template, and… |
| `@reaatech/llm-judge-infra` | published v0.1.0 | Infrastructure utilities for LLM judgment pipelines, providing a `CostTracker` with period-aware budget enforcement, a `BatchProcessor` with configurable concurrency and retry,… |
| `@reaatech/llm-judge-providers` | published v0.1.0 | A factory-pattern provider that exposes `ProviderFactory.create()` and `ProviderFactory.fromEnv()` to instantiate OpenAI, Anthropic, or local (OpenAI-compatible) LLM clients, al… |
| `@reaatech/llm-judge-templates` | published v0.1.0 | A set of evaluation prompt templates (faithfulness, relevance, coherence, safety, tool-use) that implement a `JudgmentTemplate` interface with `buildPrompt` and `parseResponse`… |
| `@reaatech/llm-judge-types` | published v0.1.0 | Shared TypeScript types, Zod schemas, and error classes for the LLM Judge Toolkit ecosystem, providing 70+ exported types and 6 typed error classes with zero runtime dependencie… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-llm-judge-toolkit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/llm-judge-toolkit
- Browse packages: https://reaatech.com/products/evals-quality/llm-judge-toolkit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, developer-tools, generative-ai, llm, openai, typescript
