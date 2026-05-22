---
name: reaatech-agent-auth-proxy
description: "These packages provide a stateful identity-aware reverse proxy that manages OAuth2 flows, API key injection, and scope enforcement for AI agents. You would adopt them to secure agent-to-service communication by centralizing credential ma…"
license: MIT
---

# REAA agent-auth-proxy

These packages provide a stateful identity-aware reverse proxy that manages OAuth2 flows, API key injection, and scope enforcement for AI agents. You would adopt them to secure agent-to-service communication by centralizing credential management and audit logging behind a zero-trust architecture. The system is built as a modular Fastify-based server paired with a shared core library and a typed client SDK, ensuring consistent schema validation and error handling across both the proxy and the agents interacting with it.

## When to use this

Reach for the `agent-auth-proxy` family when an AI agent needs to call third-party APIs on behalf of a human user and you must centralize credential management, OAuth2 token refresh, API key injection, and enforce per-request scopes. The proxy sits between the agent and the upstream service, handling the full auth lifecycle so the agent never touches raw credentials.

**Trigger phrases** that signal this family is the right fit:  
- "secure agent-to-service communication"  
- "manage OAuth2 tokens for agents"  
- "inject API keys into proxied requests"  
- "enforce scopes on outgoing requests"  
- "audit all API calls made by agents"  

Use it when the user asks for a zero‑trust architecture where the agent authenticates with an API key, gets a short‑lived JWT, and the proxy enforces allowed scopes before forwarding requests. The server is stateful (requires PostgreSQL), so it fits workflows where credential rotation and per‑user OAuth2 flows are not ephemeral.

## Quick start example

```typescript
import { AgentClient } from "@reaatech/agent-auth-proxy-client";

// The proxy base URL and the agent's API key (from environment)
const agent = new AgentClient({ baseUrl: "https://proxy.example.com", apiKey: process.env.AGENT_API_KEY! });

// Exchange API key for a JWT with requested scopes
const { token } = await agent.authenticate({ scopes: ["read:orders", "write:feedback"] });

// Make a proxied GET request to an upstream service
const response = await agent.get("/api/v2/orders", {
  headers: { Authorization: `Bearer ${token}` },
});
```

This snippet shows the core flow: authenticate with the proxy using an API key, obtain a scoped JWT, then use the client to issue proxied requests. The proxy injects the user‑level credentials and enforces scope boundaries.

## Don’t reach for this when

- **You only need to make direct API calls with a single static API key.** If no OAuth2 flow, no per‑user scoping, and no audit logging are required, just use a standard HTTP client (`fetch`, `axios`) with the key in a header. This family adds state and complexity you don’t need.

- **Your downstream API already supports direct OAuth2 from the agent.** If the agent can obtain and refresh tokens itself (e.g., via `@azure/identity` or a client credentials grant), you don’t need a proxy layer. The agent can call the API directly.

- **You need a stateless reverse proxy with no credential management.** If the only requirement is load‑balancing or URL rewriting, use nginx, Caddy, or a lightweight Fastify plugin. This family requires PostgreSQL and manages user grants.

- **Your use case demands per‑request rate limiting or caching that is not scope‑aware.** The proxy handles auth and injection, but not advanced traffic shaping. For that, pair it with a dedicated gateway (e.g., `@reaatech/api‑gateway` if available) or use something like `express‑rate‑limit`.

- **You are building a single‑user or throwaway prototype.** Spinning up PostgreSQL and the proxy server is overkill. For local experiments, hard‑code credentials or use a mock OAuth2 server.

## Packages

```bash
npm install @reaatech/agent-auth-proxy-client @reaatech/agent-auth-proxy-core @reaatech/agent-auth-proxy-server
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-auth-proxy-client` | published v1.0.0 | Two typed HTTP clients for the agent-auth-proxy server: `AgentClient` exchanges an API key for a JWT and makes proxied requests to third-party APIs on behalf of a user, while `A… |
| `@reaatech/agent-auth-proxy-core` | published v1.0.0 | Shared Zod schemas, error classes, and TypeScript types for OAuth2 proxy request validation, scope management, and error handling, exported as framework-agnostic primitives that… |
| `@reaatech/agent-auth-proxy-server` | published v1.0.0 | Fastify plugin and CLI that implements an identity-aware proxy server for agent-to-service communication, handling API key auth, OAuth2 with PKCE, JWT issuance, scope enforcemen… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-auth-proxy`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-auth-proxy
- Browse packages: https://reaatech.com/products/testing-security/agent-auth-proxy/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agent-auth, agent-proxy, agentic-ai, ai, ai-agents, developer-tools, mcp, oauth2, security, typescript
