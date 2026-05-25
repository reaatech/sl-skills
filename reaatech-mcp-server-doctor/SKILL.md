---
name: reaatech-mcp-server-doctor
description: "These packages give you a CLI tool and programmatic library that runs eight health checks against any MCP server endpoint, grades the results A–F, and outputs reports in console, JSON, markdown, or HTML formats. You would adopt them to c…"
license: MIT
---

# REAA mcp-server-doctor

These packages give you a CLI tool and programmatic library that runs eight health checks against any MCP server endpoint, grades the results A–F, and outputs reports in console, JSON, markdown, or HTML formats. You would adopt them to catch MCP server issues before deployment—transport negotiation failures, latency spikes, auth problems, or concurrency limits—and to enforce quality gates in CI. The engine, transport client, reporters, and observability instrumentation are separate packages that share core types and grading logic, so you can use just the CLI or compose the pieces programmatically (for example, running the engine with a custom transport and piping results into your own monitoring pipeline).

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
| `@reaatech/mcp-server-doctor-cli` | published v1.0.0 | A CLI that runs a full diagnostic suite against an MCP server endpoint, compares reports to detect regressions, or continuously monitors an endpoint for grade drops. It ships th… |
| `@reaatech/mcp-server-doctor-client` | published v1.0.0 | An MCP transport client that auto-negotiates between stdio, SSE, and streamable HTTP transports, providing a `createDoctorClient` factory function that returns an `MCPClient` in… |
| `@reaatech/mcp-server-doctor-core` | published v1.0.0 | Shared domain types, Zod schemas, grading benchmarks, and utility functions for the `@reaatech/mcp-server-doctor-*` diagnostic ecosystem, exporting TypeScript types (`Diagnostic… |
| `@reaatech/mcp-server-doctor-engine` | published v1.0.0 | A diagnostic engine that runs 8 health checks against an MCP server, computes a composite A–F grade, and produces a structured `DiagnosticReport`. Exports a `DiagnosticEngine` c… |
| `@reaatech/mcp-server-doctor-observability` | published v1.0.0 | A Pino-based structured logger and OpenTelemetry metrics/tracing module for MCP server diagnostics, exporting a singleton `logger`, metric recording functions (`recordCheck`, `r… |
| `@reaatech/mcp-server-doctor-reporters` | published v1.0.0 | A single dispatch function `formatReport(report, format)` that formats a `DiagnosticReport` object into console, JSON, markdown, or HTML output, with individual formatters also… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-server-doctor`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-server-doctor
- Browse packages: https://reaatech.com/products/testing-security/mcp-server-doctor/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: ai-agents, cicd, cli, developer-tools, devops, diagnostics, health-check, json-rpc, latency, load-testing, mcp, mcp-tools, model-context-protocol, nodejs, npm-package, observability, opentelemetry, profiling, testing, typescript
