---
name: reaatech-confidence-router
description: "These packages provide a decision engine that routes, clarifies, or triggers fallbacks based on the confidence scores of classification results. They solve the problem of managing ambiguity in automated systems by allowing you to chain c…"
license: MIT
---

# REAA confidence-router

These packages provide a decision engine that routes, clarifies, or triggers fallbacks based on the confidence scores of classification results. They solve the problem of managing ambiguity in automated systems by allowing you to chain classifiers—such as keyword, embedding, or LLM-based models—and optimize decision thresholds against labeled datasets. The system is built as a modular, dependency-free architecture where classifiers and language-specific prompts are registered as pluggable components within a unified routing pipeline.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Orchestration & Protocols** category. 5 packages live under `@reaatech/confidence-router` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/confidence-router @reaatech/confidence-router-classifiers @reaatech/confidence-router-core @reaatech/confidence-router-evaluation @reaatech/confidence-router-languages
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/confidence-router` | published v0.1.0 | Evaluates classification results against configurable confidence thresholds to determine whether to route to a specific target, request clarification, or trigger a fallback. It… |
| `@reaatech/confidence-router-classifiers` | published v0.1.0 | Pluggable classifier implementations for confidence-router, providing keyword |
| `@reaatech/confidence-router-core` | published v0.1.0 | Provides the core type definitions (`Prediction`, `RoutingDecision`, `RouterConfig`), a `DecisionEngine` class that thresholds predictions into route/clarify/fallback decisions,… |
| `@reaatech/confidence-router-evaluation` | published v0.1.0 | A `ThresholdOptimizer` class that performs grid search across confidence thresholds to maximize F1 score against labeled datasets, then reports accuracy, precision, recall, and… |
| `@reaatech/confidence-router-languages` | published v0.1.0 | Provides `LanguageManager` and `PromptGenerator` classes that inject 47 built-in locale configurations (including RTL |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-confidence-router`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/confidence-router
- Browse packages: https://reaatech.com/products/orchestration-protocols/confidence-router/packages
- npm scope: https://www.npmjs.com/~reaatech
