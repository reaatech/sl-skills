---
name: reaatech-mcp-server-doctor
description: "These packages provide a diagnostic suite for validating and profiling Model Context Protocol (MCP) servers. They allow you to automate health checks—covering transport negotiation, latency, and concurrency—to ensure your servers meet sp…"
license: MIT
---

# REAA mcp-server-doctor

These packages provide a diagnostic suite for validating and profiling Model Context Protocol (MCP) servers. They allow you to automate health checks—covering transport negotiation, latency, and concurrency—to ensure your servers meet specific performance and reliability standards. The system is built as a modular pipeline where a unified diagnostic engine feeds structured results into pluggable reporters and OpenTelemetry-instrumented observability tools.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **MCP Infrastructure** category. 6 packages live under `@reaatech/mcp-server-doctor-cli` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-server-doctor-cli` | published v1.0.0 | Provides a CLI for diagnosing, comparing, and monitoring Model Context Protocol (MCP) server health via stdio, SSE, or HTTP transports. It generates diagnostic reports in multip… |
| `@reaatech/mcp-server-doctor-client` | published v1.0.0 | Provides a unified client for connecting to Model Context Protocol (MCP) servers with automatic transport negotiation across stdio, SSE, and HTTP. It exposes a factory function… |
| `@reaatech/mcp-server-doctor-core` | published v1.0.0 | Provides shared TypeScript types, Zod schemas, and grading logic for building MCP server diagnostic tools. It exports utility functions for calculating latency metrics and compl… |
| `@reaatech/mcp-server-doctor-engine` | published v1.0.0 | Executes a suite of eight diagnostic checks against an MCP server to generate a structured `DiagnosticReport` with a composite A–F grade. It provides a `DiagnosticEngine` class… |
| `@reaatech/mcp-server-doctor-observability` | published v1.0.0 | Provides structured logging, OpenTelemetry metrics, and distributed tracing specifically for MCP server diagnostic checks. It exports a singleton Pino logger and a set of helper… |
| `@reaatech/mcp-server-doctor-reporters` | published v1.0.0 | Converts `DiagnosticReport` objects into console, JSON, Markdown, or HTML strings. It provides a `formatReport` dispatch function and individual formatter exports for use with d… |

## Quick start

```bash
npm install @reaatech/mcp-server-doctor-cli @reaatech/mcp-server-doctor-client @reaatech/mcp-server-doctor-core @reaatech/mcp-server-doctor-engine @reaatech/mcp-server-doctor-observability @reaatech/mcp-server-doctor-reporters
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-server-doctor`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-server-doctor
- Browse packages: https://reaatech.com/products/mcp-infrastructure/mcp-server-doctor/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: ai-agents, cicd, cli, developer-tools, devops, diagnostics, health-check, json-rpc, latency, load-testing, mcp, mcp-tools, model-context-protocol, nodejs, npm-package, observability, opentelemetry, profiling, testing, typescript
