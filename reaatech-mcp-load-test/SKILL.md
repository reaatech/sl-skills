---
name: reaatech-mcp-load-test
description: "These packages give you a purpose-built load testing framework for MCP (Model Context Protocol) servers, with a CLI, orchestration engine, transport clients, and analysis tooling. You would adopt them to stress-test MCP servers under rea…"
license: MIT
---

# REAA mcp-load-test

These packages give you a purpose-built load testing framework for MCP (Model Context Protocol) servers, with a CLI, orchestration engine, transport clients, and analysis tooling. You would adopt them to stress-test MCP servers under realistic concurrent workloads—modeling user behavior with weighted tool-call patterns, detecting breaking points, and producing latency histograms with letter grades. The framework is transport-aware, meaning it accounts for the different concurrency profiles of StreamableHTTP, SSE, and stdio, and uses session-based closed-loop concurrency where long-lived sessions continuously execute patterns with think-time delays and stateful context.

## When to use this

Reach for the mcp-load-test family when the task involves evaluating the performance, scalability, or reliability of a Model Context Protocol (MCP) server under simulated concurrent load. This family provides a modular framework for running load, ramp, soak, and spike tests, collecting latency histograms and error rates, and automatically detecting breaking points. Use it when a user mentions "load testing an MCP server" or "performance benchmarking for MCP" – especially if they want to identify maximum concurrent sessions, find throughput bottlenecks, or validate that a server meets latency SLAs under stress.

This family is also the right choice when the prompt contains "simulate multiple users with different tool-call sequences" or "generate a report card with letter grades for MCP performance". The packages are designed to work together: the client negotiates transports (stdio, SSE, StreamableHTTP), the engine orchestrates session pools with configurable concurrency profiles, the patterns executor runs stateful multi-step sequences, and the analysis package assigns grades based on latency, error rate, and recovery time. The CLI package wraps most of this into a single command for quick runs.

Pick this family over general-purpose load test tools (like k6 or artillery) when the target is specifically an MCP server – these packages understand JSON-RPC handshakes, tool discovery, and MCP-specific error codes, and produce metrics and reports tailored to MCP use cases.

## Quick start example

The most common usage is running a ramp test programmatically via the engine. The following snippet shows a ramp test that starts with 1 virtual user and ramps to 10 over 30 seconds, using a single tool call pattern.

```typescript
import { LoadEngine } from '@reaatech/mcp-load-test-engine';
import { rampProfile } from '@reaatech/mcp-load-test-profiles';
import { MetricsCollector } from '@reaatech/mcp-load-test-metrics';
import { PatternExecutor } from '@reaatech/mcp-load-test-patterns';

const engine = new LoadEngine({
  clientFactory: () => createMcpClient({ transport: 'stdio', command: 'npx', args: ['my-mcp-server'] }),
  profile: rampProfile({ startConcurrency: 1, endConcurrency: 10, durationSec: 30 }),
  pattern: new PatternExecutor({ steps: [{ tool: 'get_weather', params: { city: '$city' } }] }),
  metrics: new MetricsCollector()
});

const report = await engine.run();
console.log(report.grade, report.breakingPoint?.description);
```

## Don't reach for this when

- **You need to unit-test individual MCP tool handlers or server logic.** Use `@reaatech/mcp-testing` (if available) or a standard test framework with mocked transport. This family is for integration/load testing under concurrency, not for verifying correctness of tool implementations.
- **You want to debug a single tool call or inspect JSON-RPC messages interactively.** Use the MCP Inspector or a direct client like `@modelcontextprotocol/sdk` with raw transport logging. The load test framework hides transport details and aggregates results.
- **Your target is an HTTP API, gRPC service, or any non-MCP server.** Use general-purpose tools like k6, Artillery, or autocannon. The mcp-load-test packages are coupled to MCP's handshake and tool-discovery flow.
- **You need to run a quick smoke test with a single request and no metrics.** Just use the MCP client directly or the `mcp-cli` tool from the SDK. Setting up the full load test engine is overkill for ad-hoc single-call verification.

## Packages

```bash
npm install @reaatech/mcp-load-test-analysis @reaatech/mcp-load-test-cli @reaatech/mcp-load-test-client @reaatech/mcp-load-test-core @reaatech/mcp-load-test-engine @reaatech/mcp-load-test-metrics @reaatech/mcp-load-test-patterns @reaatech/mcp-load-test-profiles @reaatech/mcp-load-test-reporters
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-load-test-analysis` | published v0.1.0 | Provides breaking point detection and performance grading for MCP load test reports. Exports `BreakingPointDetector` (a class that monitors error rates and latency against confi… |
| `@reaatech/mcp-load-test-cli` | published v0.1.0 | A CLI that runs load, ramp, soak, and spike tests against MCP servers, outputting results to the console, Markdown, or JSON. Built on Commander.js, it accepts YAML or JSON confi… |
| `@reaatech/mcp-load-test-client` | published v0.1.0 | A session-scoped MCP transport client that auto-negotiates across stdio, SSE, and StreamableHTTP, providing `connect()`, `disconnect()`, `callTool()`, and `listTools()` methods… |
| `@reaatech/mcp-load-test-core` | published v0.1.0 | Shared TypeScript types, Zod schemas, utility functions (percentile calculation, retry with backoff, URL validation), and a pre-configured Pino logger that serve as the single s… |
| `@reaatech/mcp-load-test-engine` | published v0.1.0 | A `LoadEngine` class that orchestrates MCP server load tests by managing a dynamic session pool, executing weighted-random tool call patterns, and producing a `LoadTestReport` w… |
| `@reaatech/mcp-load-test-metrics` | published v0.1.0 | A latency histogram, throughput collector, and error tracker for MCP load testing, exposed as a `MetricsCollector` class that records per-tool percentile distributions, rolling-… |
| `@reaatech/mcp-load-test-patterns` | published v0.1.0 | A pattern execution engine and three built-in tool-call sequences (`EXPLORE_THEN_ACT`, `READ_THEN_WRITE`, `MULTI_STEP_WORKFLOW`) for load testing MCP servers, provided as a `Pat… |
| `@reaatech/mcp-load-test-profiles` | published v0.1.0 | Async generator functions that yield `{ concurrency, phase }` tuples at 1-second intervals for four load-testing profiles (ramp, soak, spike, and custom curve), designed to driv… |
| `@reaatech/mcp-load-test-reporters` | published v0.1.0 | Console, markdown, and JSON output formatters for MCP load test reports. Each reporter is a class with a `format(report: LoadTestReport): string` method, producing either ANSI-c… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-load-test`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-load-test
- Browse packages: https://reaatech.com/products/testing-security/mcp-load-test/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, cli, developer-tools, mcp, mcp-server, mcp-tools, model-context-protocol, testing, typescript
