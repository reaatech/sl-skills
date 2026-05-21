---
name: reaatech-agent-replay
description: "These packages let you record, replay, and debug AI agent interactions deterministically, using a trace-based model inspired by distributed tracing. You’d adopt them to decouple agent debugging from live LLM calls—so you can iterate with…"
license: MIT
---

# REAA agent-replay

These packages let you record, replay, and debug AI agent interactions deterministically, using a trace-based model inspired by distributed tracing. You’d adopt them to decouple agent debugging from live LLM calls—so you can iterate without burning tokens on every rerun. The core engine, provider interceptors (OpenAI, Anthropic), framework integrations (LangChain, LangGraph), and CLI compose around a shared trace schema, enabling partial replay up to a checkpoint, diff-mode comparison, and step-through debugging from a single recorded trace.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Testing & Security** category. 7 packages live under `@reaatech/agent-replay` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-replay` | published v0.1.0 | A single entry-point package that re-exports the complete public API of `@reaatech/agent-replay-core`, `@reaatech/agent-replay-interceptors`, and `@reaatech/ |
| `@reaatech/agent-replay-cli` | published v0.1.0 | A CLI for recording, replaying, debugging, and comparing AI agent traces. It provides five subcommands (`record`, `replay`, `explore`, `diff`, `debug |
| `@reaatech/agent-replay-core` | published v0.1.0 | A recording and replay engine for AI agent interactions that captures deterministic traces of LLM calls, tool invocations, and state, then replays them in stubbed, live, or part… |
| `@reaatech/agent-replay-integrations` | published v0.1.0 | Provides callback handlers and state machine hooks to record LangChain and LangGraph interactions into Agent Replay traces. It exports factory functions that generate integratio… |
| `@reaatech/agent-replay-interceptors` | published v0.1.0 | Monkey-patches OpenAI and Anthropic SDK clients to transparently record all LLM API calls (including streaming responses) into Agent Replay traces, exposing installable intercep… |
| `@reaatech/agent-replay-shared` | published v0.1.0 | Shared TypeScript types, interfaces, and error classes that define the trace data model, LLM provider abstractions, and storage contracts for the Agent Replay ecosystem. This pa… |
| `@reaatech/agent-replay-web-ui` | pending npm | Provides a web-based interface for visualizing and inspecting recorded agent traces, including span timelines, event logs, and diff comparisons. It is designed to consume trace… |

## Quick start

```bash
npm install @reaatech/agent-replay @reaatech/agent-replay-cli @reaatech/agent-replay-core @reaatech/agent-replay-integrations @reaatech/agent-replay-interceptors @reaatech/agent-replay-shared
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-replay`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-replay
- Browse packages: https://reaatech.com/products/testing-security/agent-replay/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, llm, mcp, observability, opentelemetry, replay, testing, typescript
