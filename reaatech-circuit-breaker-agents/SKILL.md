---
name: reaatech-circuit-breaker-agents
description: "These packages give you a circuit breaker designed specifically for agent-to-tool and agent-to-agent communication, with per-tool circuit isolation, confidence-aware tripping, and cost-based rate limiting. You would adopt them to prevent…"
license: MIT
---

# REAA circuit-breaker-agents

These packages give you a circuit breaker designed specifically for agent-to-tool and agent-to-agent communication, with per-tool circuit isolation, confidence-aware tripping, and cost-based rate limiting. You would adopt them to prevent cascading failures when an LLM agent repeatedly calls a degraded tool, or to automatically route traffic away from expensive or unreliable model endpoints. The most distinctive thing is that circuits are identified by a logical `circuitId` (not a hostname), and the state machine uses a lazy auto-transition model with no timers, plus optional leader-elected persistence across restarts via Firestore, DynamoDB, or Redis.

## When to use this

Reach for this package family when you're building agentic workflows that call external tools, LLMs, or other agents, and you need protection from repeated failures, runaway costs, or confidence degradation. The circuit breaker here is purpose-built for agent‑to‑tool and agent‑to‑agent communication, supporting tripping on error counts, confidence scores below a threshold, or cumulative cost over a window. It uses a lazy, timer‑free state machine that only evaluates transitions on invocation, and optionally persists state across restarts via Redis, Firestore, or DynamoDB.

Trigger phrases that signal a good fit:  
- “I need a circuit breaker for my AI agent”  
- “stop hammering a failing tool”  
- “limit costs of LLM calls”  
- “prevent cascading failures in agent tool calls”  
- “I need a smart circuit breaker that considers confidence and cost, not just errors”

Use it when the reliability concern is tied to *agentic behavior* (e.g., confidence‑based backoff) or when the circuit breaker state must survive process restarts and be shared across multiple service instances. The lazy state machine keeps overhead near zero unless a call is actually made.

## Quick start example

This example creates a simple circuit breaker from `@reaatech/circuit-breaker-core` that wraps a failing async function. The breaker trips after 3 consecutive errors and stays open for 5 seconds.

```typescript
import { CircuitBreaker } from '@reaatech/circuit-breaker-core';

const breaker = new CircuitBreaker({
  name: 'weather-tool',
  maxFailures: 3,
  resetTimeoutMs: 5000,
});

async function callWeatherAPI(location: string): Promise<string> {
  // Simulate a call that starts failing after the second request
  return breaker.call(async () => {
    const res = await fetch(`https://api.weather.com/v1/${location}`);
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    return res.text();
  });
}
```

After the third error, `callWeatherAPI` will throw a `CircuitBreakerOpenError` without touching the network until the reset timeout expires.

## Don’t reach for this when

- You only need simple retry with backoff and no state sharing. Use a dedicated retry library like `p-retry` or `async-retry`.
- You need to enforce a fixed rate limit (e.g., max 10 requests/second). This family does not control request rate – use a rate limiter such as `p-limit` or `bottleneck`.
- You’re building a standard HTTP API client with no agent‑specific logic (confidence/cost tripping). A general‑purpose circuit breaker like `opossum` or `resilience4js` is lighter.
- Your agent runs on a single process and you explicitly don’t need persistence. While the in‑memory strategy works, the core package is overkill if you never want to persist –

## Packages

```bash
npm install @reaatech/circuit-breaker-agents @reaatech/circuit-breaker-core @reaatech/circuit-breaker-persistence
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/circuit-breaker-agents` | published v0.1.1 | A meta-package that re-exports all public APIs from `@reaatech/circuit-breaker-core` and `@reaatech/circuit-breaker-persistence` as a single dependency, providing classes (`Circ… |
| `@reaatech/circuit-breaker-core` | published v0.1.1 | A circuit breaker state machine for agent-to-tool and agent-to-agent communication, providing a `CircuitBreaker` class with lazy auto-transition state evaluation, configurable t… |
| `@reaatech/circuit-breaker-persistence` | published v0.1.1 | Persistence adapters for circuit breaker state that survive restarts and share state across instances. Exports a `PersistenceAdapter` interface with four implementations: `InMem… |
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
