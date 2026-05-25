---
name: reaatech-webhook-relay-mcp
description: "These packages give you an MCP server that receives webhooks from services like Stripe, GitHub, and Twilio, normalizes them into a consistent event format, and exposes them to AI agents through subscription-based polling. You would adopt…"
license: MIT
---

# REAA webhook-relay-mcp

These packages give you an MCP server that receives webhooks from services like Stripe, GitHub, and Twilio, normalizes them into a consistent event format, and exposes them to AI agents through subscription-based polling. You would adopt them to bridge third-party webhooks into agent workflows without writing custom ingestion or validation code for each source. The packages are layered so you can run the bundled server with a single command, or import just the storage, webhook handlers, or MCP tool definitions as libraries.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **MCP Infrastructure** category. 5 packages live under `@reaatech/webhook-relay-core` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/webhook-relay-core` | pending npm | Shared types, configuration loading, AES-256-GCM encryption helpers, a filter DSL evaluator, Prometheus metric registry, and a shared pino logger for the webhook-relay-mcp proje… |
| `@reaatech/webhook-relay-mcp` | pending npm | A CLI binary (`webhook-relay-mcp`) that runs an MCP server bridging third-party webhooks (Stripe, GitHub, Replicate, Twilio, SendGrid, Slack, Vercel, and generic sources) into a… |
| `@reaatech/webhook-relay-storage` | pending npm | SQLite storage layer for webhook-relay-mcp, providing schema migrations and repository classes (Events, Subscriptions, Sources, Audit) plus services (Cleanup, Delivery with retr… |
| `@reaatech/webhook-relay-tools` | pending npm | A library that exposes an MCP server and 15 webhook management tools (subscribe, unsubscribe, list, poll, history, register, stats, replay, update-source, delete-source, rotate-… |
| `@reaatech/webhook-relay-webhooks` | pending npm | Provides webhook ingestion logic including signature validators (HMAC-SHA256/SHA1 with constant-time comparison), source handlers for Stripe, GitHub, Replicate, Twilio, SendGrid… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-webhook-relay-mcp`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/webhook-relay-mcp
- Browse packages: https://reaatech.com/products/mcp-infrastructure/webhook-relay-mcp/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, mcp, mcp-relay, mcp-server, mcp-tools, model-context-protocol, typescript, webhooks
