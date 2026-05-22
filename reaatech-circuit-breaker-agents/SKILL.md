---
name: reaatech-circuit-breaker-agents
description: "These packages provide a circuit breaker implementation designed for agent-to-tool and agent-to-agent communication, supporting confidence-based and cost-based tripping alongside standard error thresholds. You would adopt them to manage…"
license: MIT
---

# REAA circuit-breaker-agents

These packages provide a circuit breaker implementation designed for agent-to-tool and agent-to-agent communication, supporting confidence-based and cost-based tripping alongside standard error thresholds. You would adopt them to manage the reliability of LLM-based workflows by isolating tool failures and enforcing budget constraints across distributed environments. The system uses a lazy, timer-free state machine that can optionally persist state across process restarts using Firestore, DynamoDB, or Redis adapters.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Reliability & Ops** category. 7 packages live under `@reaatech/circuit-breaker-agents` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/circuit-breaker-agents @reaatech/circuit-breaker-core @reaatech/circuit-breaker-persistence
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/circuit-breaker-agents` | published v0.1.0 | Provides a unified entry point that re-exports the `CircuitBreaker` class, state machine logic, and all persistence adapters from the core and persistence sub-packages. It serve… |
| `@reaatech/circuit-breaker-core` | published v0.1.0 | A circuit breaker state machine with pluggable trip and recovery strategies for managing agent-to |
| `@reaatech/circuit-breaker-persistence` | published v0.1.0 | Provides persistence adapters for circuit breaker state to enable cross-instance state sharing and survival across process restarts. It exports a standard `PersistenceAdapter` i… |
| `@reaatech/circuit-breaker-example-basic` | pending npm | Description pending. |
| `@reaatech/circuit-breaker-example-dynamodb` | pending npm | Description pending. |
| `@reaatech/circuit-breaker-example-firestore` | pending npm | Description pending. |
| `@reaatech/circuit-breaker-example-redis` | pending npm | Description pending. |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-circuit-breaker-agents`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/circuit-breaker-agents
- Browse packages: https://reaatech.com/products/reliability-ops/circuit-breaker-agents/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, circuit-breaker, developer-tools, llm, mcp
