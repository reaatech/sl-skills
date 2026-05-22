---
name: reaatech-agent-chaos
description: "These packages provide a fault injection toolkit for testing the resilience of LLM agent systems against real-world failures like latency, rate limits, and malformed tool outputs. You would adopt these to validate that your circuit break…"
license: MIT
---

# REAA agent-chaos

These packages provide a fault injection toolkit for testing the resilience of LLM agent systems against real-world failures like latency, rate limits, and malformed tool outputs. You would adopt these to validate that your circuit breakers, fallbacks, and error-handling logic function correctly when external dependencies fail. The system uses a transparent middleware architecture that intercepts tool calls, allowing you to inject faults declaratively via YAML or JSON without modifying your agent's core implementation.

## When to use this

Reach for `agent-chaos` when the task explicitly asks to **inject faults into an LLM agent’s tool calls** to validate error-handling logic, circuit breakers, fallbacks, or retry policies. This family solves one problem: *how do you know your agent won’t crash when a tool returns a malformed JSON payload, a rate-limit error, or a 10-second delay?* You use it in test suites and staging environments, not production.

Trigger phrases that map directly to this family:
- “chaos engineering for AI agents”
- “inject latency / rate limits / malformed tool outputs”
- “validate resilience of my agent’s tool orchestration”
- “test circuit breakers or fallback behavior without mocking every tool”

The scenario-driven YAML/JSON configuration means you can codify failure modes that mirror real-world API behavior (e.g., “90% of calls return 429, then recover after 2 seconds”). The middleware architecture lets you wrap existing tool interfaces without rewriting agent code—works with LangChain, LlamaIndex, Vercel AI SDK, or any custom tool-call shape.

## Quick start example

The following loads a fault-injection scenario from YAML and wraps a single tool to inject a simulated timeout:

```typescript
import { ChaosEngine } from '@reaatech/agent-chaos-core';
import { ScenarioLoader } from '@reaatech/agent-chaos-scenarios';

// Load a scenario that injects a 5-second latency on 50% of calls
const loader = new ScenarioLoader();
const scenario = loader.loadFromYaml('./scenarios/latency-50pct.yaml');

// Wrap the original tool call with the chaos middleware
const engine = new ChaosEngine({ scenario });
const resilientTool = engine.wrapTool(originalTool);

// Use resilientTool in your agent – it will transparently inject faults
const response = await resilientTool.invoke({ query: 'fetch data' });
```

The `ChaosEngine` intercepts every tool invocation, applies the fault rules from the scenario, and records events for observability. No changes to the agent’s core logic.

## Don’t reach for this when

- **You need to unit-test a single function or pure logic.** Use standard testing frameworks (Jest, Vitest) with manual mocks. `agent-chaos` is for integration-level fault injection, not for isolated unit tests.
- **You want to simulate network failures between microservices.** Use Toxiproxy or a service mesh fault injection (e

## Packages

```bash
npm install @reaatech/agent-chaos-cli @reaatech/agent-chaos-core @reaatech/agent-chaos-scenarios
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-chaos-cli` | published v0.1.0 | Provides a CLI and programmatic API for managing, validating, and executing chaos engineering scenarios for AI agents. It supports project initialization, template generation, a… |
| `@reaatech/agent-chaos-core` | published v0.1.0 | A middleware |
| `@reaatech/agent-chaos-scenarios` | published v0.1.0 | Provides a `ScenarioLoader` class and `SchemaValidator` utility to parse, validate, and hot-reload YAML or JSON chaos injection scenarios. It supports scenario composition via i… |
| `@reaatech/agent-chaos-adapters` | pending npm | Framework adapters that wrap agent tools for LangChain, LlamaIndex, Vercel AI SDK, or any custom tool-call interface, transparently injecting faults from |
| `@reaatech/agent-chaos-e2e` | pending npm | End-to-end test suite that validates the full agent-chaos pipeline, including scenario loading, schema validation, fault injection, engine event recording, and CLI execution aga… |
| `@reaatech/agent-chaos-observability` | pending npm | Structured logging, metrics collection, OpenTelemetry tracing, and report generation for agent-chaos fault injection experiments. It provides pluggable collectors and report gen… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-chaos`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-chaos
- Browse packages: https://reaatech.com/products/testing-security/agent-chaos/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, chaos-engineering, developer-tools, fault-injection, llm, mcp, reliability, testing, typescript
