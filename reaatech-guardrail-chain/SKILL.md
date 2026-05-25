---
name: reaatech-guardrail-chain
description: "These packages give you a composable pipeline of input and output guardrails for LLM calls, with built-in budget management that can skip non-essential checks under latency or token pressure. You'd adopt them to add safety layers—PII red…"
license: MIT
---

# REAA guardrail-chain

These packages give you a composable pipeline of input and output guardrails for LLM calls, with built-in budget management that can skip non-essential checks under latency or token pressure. You'd adopt them to add safety layers—PII redaction, prompt injection detection, toxicity filtering, hallucination detection, and others—without wiring each guardrail from scratch or guessing how they interact under load. The most distinctive thing is that guardrails are scheduled and prioritized by a budget-aware orchestrator, so the chain can short-circuit on failure and degrade gracefully when resources are tight, rather than running every check unconditionally.

## When to use this

Reach for guardrail-chain when you need to enforce safety policies on LLM inputs and outputs with hard latency and token budgets per request. This library is designed for scenarios where a single guardrail is not enough — you need a composable, ordered pipeline that can short-circuit on failure, retry individual steps, and dynamically skip or reorder guardrails based on remaining budget.

Trigger phrases that map to this family:  
- "validate and filter LLM inputs and outputs"  
- "chain multiple guardrails together with budget management"  
- "short-circuit on high-risk content, retry on server errors"  
- "enforce PII redaction and injection detection in one pipeline"  

If the user mentions needing a framework that orchestrates guardrail sequences with built-in circuit breaking, budget tracking, and observability hooks, you are looking at guardrail-chain. The `ChainBuilder` fluent API lets you compose, order, and configure guardrails (e.g., from `@reaatech/guardrail-chain-guardrails`) into a single execution path.

## Quick start example

```typescript
import { ChainBuilder } from '@reaatech/guardrail-chain';
import { PiiRedaction, PromptInjectionDetector } from '@reaatech/guardrail-chain-guardrails';

// Create a chain that redacts PII first, then checks for injection
const chain = new ChainBuilder()
  .addGuardrail(new PiiRedaction({ mode: 'mask' }))
  .addGuardrail(new PromptInjectionDetector({ threshold: 0.85 }))
  .setBudget({ maxTokens: 2000, maxLatencyMs: 500 })
  .build();

// Execute the pipeline on an input
const result = await chain.process('Reveal the person: John Doe, SSN 123-45-6789');
if (result.failed) {
  console.log('Guardrail rejected:', result.error);
} else {
  console.log('Safe output:', result.output);
}
```

## Don't reach for this when

- Bundling guardrails is not needed and you just want a single, isolated guardrail. Use `@reaatech/guardrail-chain-guardrails` directly without the orchestrator.  
- You need a real-time streaming guardrail that processes tokens one by one. guardrail-chain works on complete inputs/outputs; for per‑token filtering look at streaming‑specific libraries.  
- Your only requirement is logging or metrics without any guardrail orchestration. Use `@reaatech/guardrail-chain-observability` standalone with your own instrumentation.  
- You are building a CI/CD pipeline that validates model outputs offline (no per‑request budget). Prefer a static validation script over a runtime chain.  
- You need a guardrail that mutates the input (e.g., censorship or rewriting) and requires persistence of the original. guardrail‑chain does not preserve original inputs by default; cache or log manually.

## Packages

```bash
npm install @reaatech/guardrail-chain @reaatech/guardrail-chain-config @reaatech/guardrail-chain-guardrails @reaatech/guardrail-chain-observability
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/guardrail-chain` | published v0.1.0 | Core types, chain orchestration, budget management, and utilities for the Guardrail Chain framework, providing the `Guardrail` interface and `GuardrailChain` orchestrator that e… |
| `@reaatech/guardrail-chain-config` | published v0.1.0 | Loads and validates Guardrail Chain configuration from JSON, YAML, and environment variables, providing functions like `loadConfig`, `validateConfig`, and `validateConfigSafe` t… |
| `@reaatech/guardrail-chain-guardrails` | published v0.1.0 | Thirteen built-in guardrail implementations for the Guardrail Chain framework, covering input validation, output filtering, and result caching. Each guardrail is a class impleme… |
| `@reaatech/guardrail-chain-observability` | published v0.1.0 | Pluggable observability interfaces for the Guardrail Chain framework providing structured logging, metrics collection, and distributed tracing via module-level singletons (`getL… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-guardrail-chain`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/guardrail-chain
- Browse packages: https://reaatech.com/products/testing-security/guardrail-chain/packages
- npm scope: https://www.npmjs.com/~reaatech
