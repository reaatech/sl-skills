---
name: reaatech-agent-mesh
description: "These packages provide a modular orchestrator for routing user requests to multiple AI agents using the Model Context Protocol. You would use them to build a resilient gateway that handles intent classification, session persistence, and…"
license: MIT
---

# REAA agent-mesh

These packages provide a modular orchestrator for routing user requests to multiple AI agents using the Model Context Protocol. You would use them to build a resilient gateway that handles intent classification, session persistence, and circuit breaking across a distributed set of agents. The system is designed as a collection of decoupled services that share a common set of Zod-validated types and rely on Firestore for cross-instance state management.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Orchestration & Protocols** category. 10 packages live under `@reaatech/agent-mesh` and siblings.

## Packages

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

## Quick start

```bash
npm install @reaatech/agent-mesh @reaatech/agent-mesh-classifier @reaatech/agent-mesh-confidence @reaatech/agent-mesh-gateway @reaatech/agent-mesh-mcp-server @reaatech/agent-mesh-observability @reaatech/agent-mesh-registry @reaatech/agent-mesh-router @reaatech/agent-mesh-session @reaatech/agent-mesh-utils
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-mesh`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-mesh
- Browse packages: https://reaatech.com/products/orchestration-protocols/agent-mesh/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agent-framework, agentic-ai, circuit-breaker, cloud-run, confidence-gating, contract-testing, distributed-systems, firestore, hot-reload, llm, mcp, multi-agent-orchestration, orchestrator, session-management, stateless, typescript, yaml-config
