---
name: reaatech-agent-auth-proxy
description: "These packages provide a stateful identity-aware reverse proxy that manages OAuth2 flows, API key injection, and scope enforcement for AI agents. You would adopt them to secure agent-to-service communication by centralizing credential ma…"
license: MIT
---

# REAA agent-auth-proxy

These packages provide a stateful identity-aware reverse proxy that manages OAuth2 flows, API key injection, and scope enforcement for AI agents. You would adopt them to secure agent-to-service communication by centralizing credential management and audit logging behind a zero-trust architecture. The system is built as a modular Fastify-based server paired with a shared core library and a typed client SDK, ensuring consistent schema validation and error handling across both the proxy and the agents interacting with it.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Testing & Security** category. 3 packages live under `@reaatech/agent-auth-proxy-client` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-auth-proxy-client` | published v1.0.0 | Typed HTTP client SDK with two classes: `AgentClient` for AI agents to exchange an API key for a JWT and make proxied requests, and `AdminClient` for managing users, agents, gra… |
| `@reaatech/agent-auth-proxy-core` | published v1.0.0 | Shared Zod schemas, TypeScript types, and error classes used across the agent-auth-proxy server and client SDK. Framework-agnostic and depends only on Zod for runtime validation. |
| `@reaatech/agent-auth-proxy-server` | published v1.0.0 |  |

## Quick start

```bash
npm install @reaatech/agent-auth-proxy-client @reaatech/agent-auth-proxy-core @reaatech/agent-auth-proxy-server
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-auth-proxy`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-auth-proxy
- Browse packages: https://reaatech.com/products/testing-security/agent-auth-proxy/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agent-auth, agent-proxy, agentic-ai, ai, ai-agents, developer-tools, mcp, oauth2, security, typescript
