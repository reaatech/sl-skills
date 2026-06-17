---
name: reaatech-tool-use-firewall
description: "These packages give you a policy enforcement layer that sits between an AI agent and its MCP servers, intercepting every `tools/call` to validate, rate-limit, and audit tool invocations before they reach the upstream. You'd adopt them to…"
license: MIT
---

# REAA tool-use-firewall

These packages give you a policy enforcement layer that sits between an AI agent and its MCP servers, intercepting every `tools/call` to validate, rate-limit, and audit tool invocations before they reach the upstream. You'd adopt them to prevent an agent from accidentally or maliciously executing destructive operations like `DROP TABLE` or `rm -rf /`, and to enforce budgets, approval workflows, and read-only modes across database, filesystem, or network MCP servers. The system is built as a pluggable middleware pipeline—rate limiter, cost tracker, secret scanner, argument validator, policy engine, anomaly detector, and approval workflow each implement the same `Middleware` interface and run in strict order, with stages registered only when enabled in the policy configuration.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Testing & Security** category. 7 packages live under `@reaatech/tool-use-firewall` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install tool-use-firewall @reaatech/tool-use-firewall-approvals @reaatech/tool-use-firewall-audit @reaatech/tool-use-firewall-config @reaatech/tool-use-firewall-core @reaatech/tool-use-firewall-policies @reaatech/tool-use-firewall-server
```

| Package | Status | Purpose |
| --- | --- | --- |
| `tool-use-firewall` | published v0.2.0 | A CLI and programmatic proxy server that intercepts every tool call from an AI agent to an MCP server, validating each call against a policy file before forwarding it upstream.… |
| `@reaatech/tool-use-firewall-approvals` | published v0.2.0 | A human-in-the-loop approval workflow engine for tool-use-firewall that provides an `ApprovalWorkflow` class for managing multi-level approval chains with timeouts, plus Express… |
| `@reaatech/tool-use-firewall-audit` | published v0.2.0 | An audit logger for tool-use-firewall that records ALLOW, BLOCK, and APPROVAL_REQUIRED decisions with configurable verbosity levels, automatic sensitive data redaction, rotating… |
| `@reaatech/tool-use-firewall-config` | published v0.2.0 | Zod-based policy schema definitions and YAML policy file loader for the tool-use-firewall proxy. Exports `loadPolicyConfig(path)` to read and validate a YAML policy file, `valid… |
| `@reaatech/tool-use-firewall-core` | published v0.2.0 | Core types, error classes, structured logger, sensitive data redactor, and ReDoS-safe regex utilities for the tool-use-firewall ecosystem. Exports TypeScript types (`RequestCont… |
| `@reaatech/tool-use-firewall-policies` | published v0.2.0 | A collection of middleware components—policy engine, rate limiter, cost tracker, argument validators, SQL validator, secret scanner, anomaly detector, and read-only enforcement—… |
| `@reaatech/tool-use-firewall-server` | published v0.2.0 | An MCP proxy server that spawns upstream MCP servers as child processes, intercepts JSON-RPC `tools/call` messages, runs them through a configurable policy pipeline (rate limite… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-tool-use-firewall`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/tool-use-firewall
- Browse packages: https://reaatech.com/products/testing-security/tool-use-firewall/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, llm, mcp, mcp-gateway, mcp-server, model-context-protocol, security, typescript
