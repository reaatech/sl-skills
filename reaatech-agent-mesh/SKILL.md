---
name: reaatech-agent-mesh
description: "These packages provide a modular orchestrator for routing user requests to multiple AI agents using the Model Context Protocol. You would use them to build a resilient gateway that handles intent classification, session persistence, and…"
license: MIT
---

# REAA agent-mesh

These packages provide a modular orchestrator for routing user requests to multiple AI agents using the Model Context Protocol. You would use them to build a resilient gateway that handles intent classification, session persistence, and circuit breaking across a distributed set of agents. The system is designed as a collection of decoupled services that share a common set of Zod-validated types and rely on Firestore for cross-instance state management.

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
| `@reaatech/agent-mesh` | published v1.0.0 | Provides core domain types, Zod validation schemas, environment configuration, and shared constants for the @reaatech/agent-mesh multi-agent orchestrator. It exports 15+ TypeScr… |
| `@reaatech/agent-mesh-classifier` | published v1.0.0 | Classifies user input against an agent registry using Gemini Flash, returning a structured object containing the target agent ID, confidence score, and intent summary. It provid… |
| `@reaatech/agent-mesh-confidence` | published v1.0.0 | Evaluates routing decisions for an agent-mesh orchestrator using a deterministic decision tree that compares classifier confidence against agent-specific thresholds. It provides… |
| `@reaatech/agent-mesh-gateway` | published v1.0.0 | Express middleware and request handlers that expose `/v1/ |
| `@reaatech/agent-mesh-mcp-server` | published v1.0.0 | Exposes an agent-mesh orchestrator as an MCP-compliant server by providing Express middleware and handlers for JSON-RPC 2.0 routing and SSE transport. It registers tools to rout… |
| `@reaatech/agent-mesh-observability` | published v1.0.0 | Provides structured JSON logging with automatic PII redaction, OpenTelemetry tracing and metrics, and audit event logging for agent-mesh orchestrators—exposes a Winston logger,… |
| `@reaatech/agent-mesh-registry` | published v1.0.0 | Manages a thread-safe registry of validated YAML agent configurations, providing atomic-swap updates and SIGHUP-triggered hot-reloading. It exposes a singleton state object for… |
| `@reaatech/agent-mesh-router` | published v1.0.0 | Provides dispatch and validation functions for routing requests to MCP-based agents over |
| `@reaatech/agent-mesh-session` | published v1.0.0 | Manages multi-turn conversation state and workflow context using Firestore as a persistence layer. It provides a set of asynchronous functions for session lifecycle management,… |
| `@reaatech/agent-mesh-utils` | published v1.0.0 | Provides a three-state circuit breaker for managing agent health, featuring Firestore-backed persistence and leader election for cross-instance state synchronization. It exposes… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-mesh`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-mesh
- Browse packages: https://reaatech.com/products/orchestration-protocols/agent-mesh/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agent-framework, agentic-ai, circuit-breaker, cloud-run, confidence-gating, contract-testing, distributed-systems, firestore, hot-reload, llm, mcp, multi-agent-orchestration, orchestrator, session-management, stateless, typescript, yaml-config
