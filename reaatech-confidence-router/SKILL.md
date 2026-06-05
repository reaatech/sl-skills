---
name: reaatech-confidence-router
description: "These packages give you a decision engine that turns classifier confidence scores into one of three actions: route to a handler, ask the user for clarification, or fall back to a default. You'd adopt them to handle ambiguous or low-confi…"
license: MIT
---

# REAA confidence-router

These packages give you a decision engine that turns classifier confidence scores into one of three actions: route to a handler, ask the user for clarification, or fall back to a default. You'd adopt them to handle ambiguous or low-confidence predictions in a conversational or routing system without hard-coding every edge case. The most distinctive thing is the threshold-based triage model—you set two confidence boundaries and the engine automatically decides whether to proceed, ask, or bail, with pluggable classifiers (keyword, embedding, LLM) that chain in priority order.

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
| `@reaatech/confidence-router` | published v0.1.1 | A threshold-based decision engine that takes a classification result with confidence scores and returns a `RoutingDecision` indicating whether to **route** (high confidence), **… |
| `@reaatech/confidence-router-classifiers` | published v0.1.1 | A pluggable classifier system for confidence-router, providing keyword matching, embedding similarity, and LLM-based classification as classes conforming to the `Classifier` int… |
| `@reaatech/confidence-router-core` | published v0.1.1 | Core type definitions, error classes, configuration utilities, and the `DecisionEngine` for the confidence-router ecosystem. Exports TypeScript types (`Prediction`, `RoutingDeci… |
| `@reaatech/confidence-router-evaluation` | published v0.1.1 | A grid search optimizer that tunes `routeThreshold` and `fallbackThreshold` on any `RouterInterface` object to maximize F1 score against a labeled dataset, returning `OptimizedT… |
| `@reaatech/confidence-router-languages` | published v0.1.1 | Provides locale-aware clarification prompt generation for confidence-router, exposing `LanguageManager` and `PromptGenerator` classes that handle 47 built-in languages with RTL… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-confidence-router`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/confidence-router
- Browse packages: https://reaatech.com/products/orchestration-protocols/confidence-router/packages
- npm scope: https://www.npmjs.com/~reaatech
