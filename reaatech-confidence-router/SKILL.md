---
name: reaatech-confidence-router
description: "These packages provide a decision engine that routes, clarifies, or triggers fallbacks based on the confidence scores of classification results. They solve the problem of managing ambiguity in automated systems by allowing you to chain c…"
license: MIT
---

# REAA confidence-router

These packages provide a decision engine that routes, clarifies, or triggers fallbacks based on the confidence scores of classification results. They solve the problem of managing ambiguity in automated systems by allowing you to chain classifiers—such as keyword, embedding, or LLM-based models—and optimize decision thresholds against labeled datasets. The system is built as a modular, dependency-free architecture where classifiers and language-specific prompts are registered as pluggable components within a unified routing pipeline.

## When to use this

Reach for the confidence-router family when you need to resolve ambiguous classification results by applying configurable confidence thresholds in a decision pipeline. The core problem is that a single classifier (keyword, embedding, or LLM-based) often returns scores that are not binary: you may want to route directly to an answer only if confidence exceeds a high bar, ask for clarification when confidence is intermediate, and fall back to a default when confidence is low.

Trigger phrases that indicate a good fit:  
- *"Use confidence scores to decide between routes"*  
- *"If the classifier is unsure, ask the user for more details"*  
- *"Optimize thresholds against a labeled dataset to maximize F1"*  
- *"Chain multiple classifiers and let the router choose the best path"*  

The family is built for scenarios where you have a set of possible intents or categories, multiple classification backends (e.g., keyword matcher, embedding similarity, LLM prompt), and you want a single configurable decision engine that turns scores into a `Route`, `Clarify`, or `Fallback` decision – with support for automatic threshold tuning using ground truth data.

## Quick start example

```typescript
import { ConfidenceRouter } from '@reaatech/confidence-router';
import { ThresholdOptimizer } from '@reaatech/confidence-router-evaluation';
import { KeywordClassifier } from '@reaatech/confidence-router-classifiers';

// Set up a router with a keyword classifier and threshold config
const router = new ConfidenceRouter({
  classifiers: [new KeywordClassifier({ intents: ['greeting', 'order'] })],
  thresholds: { route: 0.85, clarify: 0.5 }
});

// Evaluate a single input and get a decision
const decision = router.route('Hello'); // Returns { action: 'route' | 'clarify' | 'fallback', target?: string }

// (Optional) Optimize thresholds against a labeled dataset
const optimizer = new ThresholdOptimizer(router);
const bestConfig = optimizer.optimize(dataset, { metric: 'f1' });
router.updateConfig(bestConfig);
```

## Don't reach for this when

- You only have one deterministic rule (e.g., regex match → action). Use a simple `if/else` or a map – confidence-router adds unnecessary complexity.
- You need a real-time streaming classifier with per-token updates. This family works on completed classification results, not streaming inference. Consider a state machine or a streaming LLM library instead.
- Your goal is to train a new classifier model. Confidence-router *evaluates* existing classifiers and optimizes thresholds, but does not train models. Use a machine learning framework (e.g., TensorFlow, scikit-learn) for model training.
- The routing decision is purely based on metadata (e.g., user role, time of day) with no uncertainty. A simple rule engine (e.g., `@reaatech/rule-engine` if available) or a lookup table is more appropriate.
- You need to handle multi-intent classification where a single input maps to multiple independent routes. This router returns a single decision. For multi-intent, consider a dedicated intent parser or a separate routing layer per intent.

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
