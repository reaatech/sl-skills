---
name: reaatech-agent-mesh
description: "These packages give you a complete multi-agent orchestrator that routes user requests to the right agent based on intent confidence, manages multi-turn sessions, and isolates failing agents with circuit breakers. You would adopt them to…"
license: MIT
---

# REAA agent-mesh

These packages give you a complete multi-agent orchestrator that routes user requests to the right agent based on intent confidence, manages multi-turn sessions, and isolates failing agents with circuit breakers. You would adopt them to build a production system where multiple specialized AI agents handle different tasks (like password resets, HR queries, or IT support) behind a single API endpoint, with automatic fallback and clarification when the intent is unclear. The most distinctive thing is how the packages compose around a confidence-gated decision tree—Gemini Flash classifies intent, a 5-rule engine decides whether to route, clarify, or fall back, and per-agent circuit breakers with Firestore persistence prevent cascading failures across Cloud Run instances.

## When to use this

Reach for the `@reaatech/agent-mesh` family when you need to build a production-grade gateway that routes user requests to multiple AI agents over the Model Context Protocol (MCP). This is the right choice when the task requires **intent classification** of free-form user input, **confidence gating** to decide whether to route or ask for clarification, **session persistence** across distributed instances (via Firestore), and **circuit breaking** to protect unhealthy agents. The family is designed for multi-agent orchestration where agents are independently deployed and their configurations must be hot-reloadable without service restart.

Common trigger phrases include: "multi-agent orchestrator", "routing user requests to AI agents", "gateway with circuit breaker", "distributed session state", "hot-reloadable agent configs", and "confidence-based fallback". If the user describes a system that classifies user intent against a list of agents, maintains conversation history across Cloud Run instances, and gracefully degrades when agents fail, this family is the right fit.

The family is modular – you can use only the packages you need (e.g., just `@reaatech/agent-mesh-classifier` and `@reaatech/agent-mesh-confidence` if you already have a gateway). All packages share Zod-validated types from `@reaatech/agent-mesh` and rely on Firestore for cross-instance state, making them suitable for horizontally scaled serverless deployments.

## Quick start example

This example demonstrates the core flow: classify a user message, check confidence, retrieve or create a session, and route the request to the intended agent.

```typescript
import { AgentRequest, SessionState } from '@reaatech/agent-mesh';
import { classify } from '@reaatech/agent-mesh-classifier';
import { evaluateConfidence } from '@reaatech/agent-mesh-confidence';
import { getOrCreateSession } from '@reaatech/agent-mesh-session';
import { dispatch } from '@reaatech/agent-mesh-router';

async function handleUserMessage(userId: string, message: string) {
  const classification = await classify(message);
  const decision = evaluateConfidence(classification);
  if (decision.action === 'clarify') {
    return decision.clarificationQuestion;
  }
  const session = await getOrCreateSession(userId, classification.agentId);
  const response = await dispatch(session.id, message, classification.agentId);
  return response;
}
```

## Don't reach for this when

- **You only have a single agent and no need for intent classification.** Use a direct MCP client or a simple wrapper like `@modelcontextprotocol/sdk` to reduce complexity.
- **Your agents are stateless and session persistence is not required.** The Firestore dependency adds latency and cost; consider an in-memory store (e.g., `Map`) or a lightweight solution like `better-sqlite3` for single-instance deployments.
- **You don't need cross-instance resilience features (circuit breaker, leader election).** The circuit breaker and session packages assume distributed state; for a single Cloud Run instance, simpler retry logic and local state suffice.
- **You need a generic API gateway that proxies to arbitrary LLM providers (OpenAI, Anthropic, etc.).** This family is purpose-built for MCP-based agents. For a vendor-agnostic LLM gateway, look at LiteLLM or a dedicated proxy like `openai-gateway`.
- **Your use case involves only human-in-the-loop validation or approval workflows.** The session and routing logic here is designed for autonomous agentic flows. For approval steps, consider a dedicated state machine library (e.g., `xstate`) or a workflow system.

## Packages

```bash
npm install @reaatech/agent-mesh @reaatech/agent-mesh-classifier @reaatech/agent-mesh-confidence @reaatech/agent-mesh-gateway @reaatech/agent-mesh-mcp-server @reaatech/agent-mesh-observability @reaatech/agent-mesh-registry @reaatech/agent-mesh-router @reaatech/agent-mesh-session @reaatech/agent-mesh-utils
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-mesh` | published v1.0.0 | A Zod-schema and TypeScript type package that defines the core domain entities, request/response shapes, and environment configuration for the agent-mesh multi-agent orchestrato… |
| `@reaatech/agent-mesh-classifier` | published v1.0.0 | A Gemini Flash intent classifier that maps user requests to registered agents, returning a `ClassifierOutput` with agent ID, confidence score, and detected language. It dynamica… |
| `@reaatech/agent-mesh-confidence` | published v1.0.0 | A decision engine that evaluates classifier output against agent thresholds using a 5-rule decision tree to route requests, ask for clarification, or fall back to a default agen… |
| `@reaatech/agent-mesh-gateway` | published v1.0.0 | An Express middleware pipeline that provides a `/v1/request` endpoint for orchestrating agent dispatch, including authentication, rate limiting, TLS enforcement, health checks,… |
| `@reaatech/agent-mesh-mcp-server` | published v1.0.0 | Exposes the agent-mesh orchestrator as an MCP-compliant agent by providing Express middleware (`mcpMiddleware`), JSON-RPC 2.0 handlers, and SSE transport for legacy client compa… |
| `@reaatech/agent-mesh-observability` | published v1.0.0 | A structured observability layer for the agent-mesh orchestrator, providing Winston-powered JSON logging with automatic PII redaction, OpenTelemetry tracing and metrics (histogr… |
| `@reaatech/agent-mesh-registry` | published v1.0.0 | A YAML-based agent registry loader that parses, validates (with Zod), and hot-reloads agent configurations via SIGHUP, exposing a thread-safe singleton with atomic-swap semantic… |
| `@reaatech/agent-mesh-router` | published v1.0.0 | A Zod-validated MCP dispatch layer that routes requests to registered agents via StreamableHTTP transport, managing connection pooling, circuit breakers, retries, and timeouts p… |
| `@reaatech/agent-mesh-session` | published v1.0.0 | Firestore-backed session management for the agent-mesh orchestrator, providing functions (`createSession`, `getActiveSession`, `appendTurn`, `closeSession`, `resumeSession`) to… |
| `@reaatech/agent-mesh-utils` | published v1.0.0 | A per-agent circuit breaker with Firestore-backed persistence and leader-elected cross-instance state synchronization, exposed as a singleton object with synchronous methods (`c… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-mesh`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-mesh
- Browse packages: https://reaatech.com/products/orchestration-protocols/agent-mesh/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agent-framework, agentic-ai, circuit-breaker, cloud-run, confidence-gating, contract-testing, distributed-systems, firestore, hot-reload, llm, mcp, multi-agent-orchestration, orchestrator, session-management, stateless, typescript, yaml-config
