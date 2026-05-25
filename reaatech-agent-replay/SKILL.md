---
name: reaatech-agent-replay
description: "These packages give you a deterministic recording and replay system for AI agent interactions. You'd adopt them to debug agent behavior without burning LLM tokens on every iteration — record a trace once, then replay it in stubbed, parti…"
license: MIT
---

# REAA agent-replay

These packages give you a deterministic recording and replay system for AI agent interactions. You'd adopt them to debug agent behavior without burning LLM tokens on every iteration — record a trace once, then replay it in stubbed, partial, or diff modes for zero-cost debugging and regression testing. The system is built around a trace-based data model with hierarchical spans and events, with interceptors that monkey-patch OpenAI and Anthropic SDKs transparently, so recording happens without modifying your agent code.

## When to use this

Reach for the agent-replay family when you need to record AI agent interactions and replay them deterministically without issuing real LLM API calls. This is the right toolkit for decoupling development and testing from live token consumption — capture a trace once, then iterate on logic, prompts, or tool definitions offline.

Trigger phrases that map to this family:

- “We need to replay agent runs without calling the API again” — use `ReplayEngine` from `@reaatech/agent-replay-core` to load a previously recorded trace.
- “Debug step-by-step through an agent’s LLM calls and tool uses” — the `ReplayDebugger` lets you inspect spans and state per checkpoint.
- “Compare two agent runs to find regressions” — `DiffEngine` from the same core package outputs structured diffs between traces.
- “Instrument LangChain or LangGraph agents for replay” — use `@reaatech/agent-replay-integrations` to hook into framework callbacks.

The family solves offline testing, regression detection, and deterministic debugging of agentic workflows. Any task that mentions recording LLM interactions, stubbing API calls, or trace-based debugging during CI is a candidate.

## Quick start example

The most common flow: create a `RecordingEngine`, install an interceptor to capture OpenAI calls, run your agent, then save the trace for later replay. The `@reaatech/agent-replay` entry-point re-exports everything needed.

```typescript
import { RecordingEngine, ReplayEngine, OpenAIInterceptor } from '@reaatech/agent-replay';

const recorder = new RecordingEngine();
const interceptor = new OpenAIInterceptor(recorder);

// Interceptor patches the OpenAI client transparently
interceptor.install();

// Run your agent code that calls OpenAI
await myAgent.run('What is the weather in Berlin?');

// Retrieve the recorded trace
const trace = recorder.getTrace();

// Later, replay the same sequence without real API calls
const replay = new ReplayEngine(trace);
const replayedSteps = await replay.replayAll(); // returns stubbed responses
```

## Don't reach for this when

- **When you need to capture arbitrary HTTP requests or non-LLM API calls.**  
  Agent-replay focuses on LLM and tool invocation traces. For general HTTP recording, use a dedicated library like `nock` or a proxy-based recorder such as Polly.js.

- **When you want to test LLM output quality (e.g., semantic correctness) against live responses.**  
  Replaying stubbed data won't evaluate model behavior. Use a separate evaluation framework (e.g., `@reaatech/eval-models`) that calls real endpoints and scores outputs.

- **When you only need to mock a single OpenAI response in a unit test.**  
  Direct mocking (`jest.spyOn` or `sinon.stub`) is lighter. Agent-replay is designed for multi-step, stateful workflows; for a one-off mock, a simpler stub suffices.

- **When you must replay across different LLM providers not in the interceptor list.**  
  Currently only OpenAI and Anthropic SDKs are supported. If your agent uses Cohere, Mistral, or a custom endpoint, you’ll need to extend the interceptor or use the core `RecordingEngine` API manually to instrument each call.

## Packages

```bash
npm install @reaatech/agent-replay @reaatech/agent-replay-cli @reaatech/agent-replay-core @reaatech/agent-replay-integrations @reaatech/agent-replay-interceptors @reaatech/agent-replay-shared
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-replay` | published v0.1.0 | A convenience re-export package that provides the complete public API of the Agent Replay ecosystem—core engine, LLM interceptors, and shared types—from a single import. |
| `@reaatech/agent-replay-cli` | published v0.1.0 | A CLI tool for recording, replaying, debugging, and comparing AI agent traces, providing five subcommands (`record`, `replay`, `explore`, `diff`, `debug`) that operate on `.artr… |
| `@reaatech/agent-replay-core` | published v0.1.0 | Recording and replay engine for AI agent traces, providing a `RecordingEngine` to capture span-structured interactions with events and checkpoints, and a `ReplayEngine` to deter… |
| `@reaatech/agent-replay-integrations` | published v0.1.0 | Provides callback handlers (`createLangChainHandler`) and state machine hooks (`createLangGraphHooks`) that record LangChain and LangGraph agent interactions into traces, return… |
| `@reaatech/agent-replay-interceptors` | published v0.1.0 | Monkey-patches OpenAI and Anthropic SDK clients to transparently record all LLM API calls into Agent Replay traces, providing `OpenAIInterceptor` and `AnthropicInterceptor` clas… |
| `@reaatech/agent-replay-shared` | published v0.1.0 | Shared types, interfaces, error classes, and configuration for the Agent Replay ecosystem, providing the canonical trace model shapes, LLM provider abstractions, and storage con… |
| `@reaatech/agent-replay-web-ui` | pending npm | Web-based trace viewer for Agent Replay that provides an interactive UI for exploring recorded agent traces, including span timelines, event inspection, checkpoint navigation, a… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-replay`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-replay
- Browse packages: https://reaatech.com/products/evals-quality/agent-replay/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, llm, mcp, observability, opentelemetry, replay, testing, typescript
