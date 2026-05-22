---
name: reaatech-mcp-server-doctor
description: "These packages provide a diagnostic suite for validating and profiling Model Context Protocol (MCP) servers. They allow you to automate health checks—covering transport negotiation, latency, and concurrency—to ensure your servers meet sp…"
license: MIT
---

# REAA mcp-server-doctor

These packages provide a diagnostic suite for validating and profiling Model Context Protocol (MCP) servers. They allow you to automate health checks—covering transport negotiation, latency, and concurrency—to ensure your servers meet specific performance and reliability standards. The system is built as a modular pipeline where a unified diagnostic engine feeds structured results into pluggable reporters and OpenTelemetry-instrumented observability tools.

## When to use this

Reach for this package family whenever a task asks to **validate, profile, or grade the health of a Model Context Protocol (MCP) server**. The family automates transport negotiation checks, latency profiling, concurrency stress testing, and schema compliance, producing a structured `DiagnosticReport` with an A–F grade. Use it to embed MCP server diagnostics into CI/CD pipelines, CLI tooling, or release gates.

Trigger phrases that should pull this family:  
- "Run a health check on the MCP server"  
- "Get a diagnostic report for the MCP endpoint"  
- "Compare two diagnostic runs to catch regression"  
- "Grade the MCP server's latency and concurrency"  

The core engine (`@reaatech/mcp-server-doctor-engine`) consumes an initialized MCP client from `@reaatech/mcp-server-doctor-client` and runs eight predefined checks. Pluggable reporters (`@reaatech/mcp-server-doctor-reporters`) convert results into console, JSON, Markdown, or HTML. The observability package (`@reaatech/mcp-server-doctor-observability`) adds structured logging and OpenTelemetry metrics that activate only when environment variables are set, keeping production use safe.

## Quick start example

```typescript
import { createClient } from '@reaatech/mcp-server-doctor-client'
import { DiagnosticEngine } from '@reaatech/mcp-server-doctor-engine'
import { formatReport } from '@reaatech/mcp-server-doctor-reporters'

async function diagnoseServer(serverUrl: string) {
  const client = await createClient({ transport: 'sse', url: serverUrl })
  const engine = new DiagnosticEngine(client, { latencyThresholdMs: 500, concurrencyWorkers: 10 })
  const report = await engine.diagnose()
  console.log(formatReport(report, 'console'))
  return report.grade
}
```

This snippet connects to an SSE MCP server, runs the full diagnostic suite, and prints the formatted report to the console. The grade (`A`–`F`) is available for programmatic decision-making.

## Don't reach for this when

- **You need to build or host an MCP server.** This family is a diagnostic tool, not a server framework. Use `@reaatech/mcp-server` packages or a third-party MCP server implementation.
- **You are performing generic HTTP endpoint health checks (non-MCP).** The diagnostic engine expects an MCP client with tool discovery and JSON-RPC methods. For plain REST/HTTP health, use a library like `axios` with custom assertions or a dedicated health-check library.
- **You want real-time, continuous monitoring of a production MCP server.** The engine is designed for one-shot or scheduled batched diagnostics, not for streaming metrics. Pair it with a cron job or CI trigger, but for live dashboards instrument the server itself with OpenTelemetry.
- **You only need transport negotiation without running the full diagnostic suite.** The `@reaatech/mcp-server-doctor-client` package alone can handle connection and negotiation; you do not need the engine or reporters.
- **You are writing tests that mock the MCP protocol.** The diagnostic engine works against a real MCP client. For mocking in unit tests, use a mock transport or a dedicated MCP client mock library.

## Packages

```bash
npm install @reaatech/mcp-server-doctor-cli @reaatech/mcp-server-doctor-client @reaatech/mcp-server-doctor-core @reaatech/mcp-server-doctor-engine @reaatech/mcp-server-doctor-observability @reaatech/mcp-server-doctor-reporters
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-server-doctor-cli` | published v1.0.0 | Provides a CLI for diagnosing, comparing, and monitoring Model Context Protocol (MCP) server health via stdio, SSE, or HTTP transports. It generates diagnostic reports in multip… |
| `@reaatech/mcp-server-doctor-client` | published v1.0.0 | Provides a unified client for connecting to Model Context Protocol (MCP) servers with automatic transport negotiation across stdio, SSE, and HTTP. It exposes a factory function… |
| `@reaatech/mcp-server-doctor-core` | published v1.0.0 | Provides shared TypeScript types, Zod schemas, and grading logic for building MCP server diagnostic tools. It exports utility functions for calculating latency metrics and compl… |
| `@reaatech/mcp-server-doctor-engine` | published v1.0.0 | Executes a suite of eight diagnostic checks against an MCP server to generate a structured `DiagnosticReport` with a composite A–F grade. It provides a `DiagnosticEngine` class… |
| `@reaatech/mcp-server-doctor-observability` | published v1.0.0 | Provides structured logging, OpenTelemetry metrics, and distributed tracing specifically for MCP server diagnostic checks. It exports a singleton Pino logger and a set of helper… |
| `@reaatech/mcp-server-doctor-reporters` | published v1.0.0 | Converts `DiagnosticReport` objects into console, JSON, Markdown, or HTML strings. It provides a `formatReport` dispatch function and individual formatter exports for use with d… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-server-doctor`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-server-doctor
- Browse packages: https://reaatech.com/products/mcp-infrastructure/mcp-server-doctor/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: ai-agents, cicd, cli, developer-tools, devops, diagnostics, health-check, json-rpc, latency, load-testing, mcp, mcp-tools, model-context-protocol, nodejs, npm-package, observability, opentelemetry, profiling, testing, typescript
