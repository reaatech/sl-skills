---
name: reaatech-prompt-injection-bench
description: "These packages give you a reproducible benchmark for evaluating prompt-injection defenses in AI agent systems, including an attack corpus with 300+ templates across 8 categories, pluggable defense adapters (Rebuff, Lakera Guard, LLM Guar…"
license: MIT
---

# REAA prompt-injection-bench

These packages give you a reproducible benchmark for evaluating prompt-injection defenses in AI agent systems, including an attack corpus with 300+ templates across 8 categories, pluggable defense adapters (Rebuff, Lakera Guard, LLM Guard, Garak, OpenAI/Azure/Anthropic/Cohere Moderation, Custom HTTP), a scoring engine with statistical analysis, a public leaderboard, and an MCP server. You would adopt them to objectively measure and compare how well different defenses detect and block prompt injection attacks, using a standardized methodology with deterministic seeds and SHA-256 proofs for reproducibility. The packages are designed as independent modules—core types, corpus builder, adapters, runner, scoring, leaderboard, observability, and MCP server—that share canonical Zod schemas and a common `DefenseAdapter` interface, so you can mix and match only the pieces you need or run the full benchmark via the umbrella CLI.

## When to use this

Reach for this family when the task involves measuring how well a prompt injection defense works: detection rates, false positive rates, or statistical significance. The packages are designed for structured, reproducible benchmarking — not for running defenses in production. Use them when the user explicitly asks to “benchmark a defense against an injection corpus”, “compare two detection methods on the same attack suite”, or “generate a leaderboard with confidence intervals”. The shape of the request usually includes phrases like “run adversarial tests”, “evaluate security posture”, “need a standardized benchmark for prompt injection”, or “calculate attack success rates”. This is the right choice when the deliverable is a quantitative report (e.g., detection accuracy, Wilson score, Cohen’s h) across multiple attack categories, with configurable seeds for reproducibility.

Also reach for it when the user wants to integrate benchmarking into an automated pipeline or MCP toolchain. The `pi-bench-mcp-server` package exposes all operations as MCP tools, so agents can execute benchmarks and compare results via a server interface. If the user says “trigger a benchmark from a workflow” or “return structured benchmark results as JSON”, this family provides the standardized schema and runner.

## Quick start example

The following programmatic example uses the main `prompt-injection-bench` library to create an in-memory defense adapter, load a default attack corpus, run a single benchmark, and print summary scores. This mirrors the CLI's default behavior but is embeddable in tests or automation scripts.

```typescript
import { runBenchmark, DefenseAdapter, AttackCorpus } from 'prompt-injection-bench';

class PassThroughAdapter extends DefenseAdapter {
  async detect(input: string): Promise<{ isInjection: boolean; confidence: number }> {
    return { isInjection: false, confidence: 0 };
  }
  async sanitize(input: string): Promise<string> { return input; }
}

async function main() {
  const corpus = AttackCorpus.default();
  const results = await runBenchmark({
    adapter: new PassThroughAdapter(),
    corpus,
    iterations: 100,
    seed: 42,
  });
  console.log(results.summary());
}
main();
```

## Don't reach for this when

- **You need real-time, production-grade injection detection.** This family benchmarks the *defense itself* — it does not provide a runtime detection engine. For that, use a dedicated defense library such as `@reaatech/pi-bench-adapters` only as a wrapper around an existing service, or a third-party detection API.
- **You want to generate adversarial examples for training or fine-tuning.** The corpus package (`pi-bench-corpus`) is for benchmarking, not for augmenting training data. For adversarial training, look at libraries like `textattack` or `robustness` that perturb actual model inputs.
- **You are testing non-text models (vision, audio) or non-LLM systems.** The attack taxonomy and scoring are specific to text-based prompt injection. For other modalities, use a domain-specific adversarial testing framework.
- **You need to defend a production system against injection in real time.** The benchmark runner does not deploy any defense — it only measures it. For runtime protection, integrate a dedicated injection detection middleware (e.g., from an LLM security gateway).
- **You only want a list of known attack templates without the benchmarking pipeline.** The corpus package is part of a larger pipeline. If you just need a static JSON list of prompts, consider the `@reaatech/pi-bench-corpus` package alone, but note it expects to be used with the schema from `pi-bench-core`. For a simpler corpus, extract the templates manually or use a third-party list like “Prompt Injection Dataset” on Hugging Face.

## Packages

```bash
npm install @reaatech/pi-bench-adapters @reaatech/pi-bench-core @reaatech/pi-bench-corpus @reaatech/pi-bench-leaderboard @reaatech/pi-bench-mcp-server @reaatech/pi-bench-observability @reaatech/pi-bench-runner @reaatech/pi-bench-scoring prompt-injection-bench
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/pi-bench-adapters` | published v1.0.1 | A collection of pluggable defense adapters for prompt injection detection, providing a standard `DefenseAdapter` interface with `detect()` and `sanitize()` methods across 8 buil… |
| `@reaatech/pi-bench-core` | published v1.0.1 | Canonical TypeScript types, Zod schemas, and attack taxonomy for the prompt-injection-bench benchmarking suite. Exports 18 domain types, 16 Zod schemas for runtime validation, a… |
| `@reaatech/pi-bench-corpus` | published v1.0.1 | A corpus builder, validator, and variant generation engine for prompt-injection-bench that produces balanced injection samples from YAML templates using synonym replacement, Uni… |
| `@reaatech/pi-bench-leaderboard` | published v1.0.1 | A leaderboard manager for prompt-injection-bench that ranks defenses by composite score into S/A/B/C/D tiers, with JSON file persistence and pairwise comparison. Exports a `crea… |
| `@reaatech/pi-bench-mcp-server` | published v1.0.1 | An MCP server that exposes four tools (`run_benchmark`, `compare_defenses`, `generate_report`, `submit_results`) for running prompt injection benchmarks against defenses, compar… |
| `@reaatech/pi-bench-observability` | published v1.0.1 | A structured logging, OpenTelemetry-compatible metrics, and span-based tracing toolkit for benchmark operations, built on Pino v10 and the OpenTelemetry SDK. It provides `create… |
| `@reaatech/pi-bench-runner` | published v1.0.1 | A benchmark execution engine that runs prompt injection attacks against defense adapters in parallel with configurable timeouts, progress reporting, and PII-safe result collecti… |
| `@reaatech/pi-bench-scoring` | published v1.0.1 | Computes weighted defense scores, confidence intervals, effect sizes, and statistical comparisons (z-test, chi-square, ANOVA) for prompt-injection benchmark results, exporting f… |
| `prompt-injection-bench` | published v1.0.1 | A CLI and library for benchmarking LLM prompt-injection defenses against standardized attack corpora. Exports a `createBenchmarkEngine` function and CLI with subcommands (`bench… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-prompt-injection-bench`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/prompt-injection-bench
- Browse packages: https://reaatech.com/products/testing-security/prompt-injection-bench/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: adversarial-testing, agentic-ai, ai-safety, ai-security, benchmark, defense, llm, llm-security, prompt-injection, red-teaming, sanitization, security, test-corpus, typescript
