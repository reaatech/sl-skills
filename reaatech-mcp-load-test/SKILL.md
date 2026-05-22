---
name: reaatech-mcp-load-test
description: "These packages provide a load testing framework designed to evaluate the performance and reliability of Model Context Protocol (MCP) servers. You would use them to simulate concurrent user behavior, identify breaking points, and generate…"
license: MIT
---

# REAA mcp-load-test

These packages provide a load testing framework designed to evaluate the performance and reliability of Model Context Protocol (MCP) servers. You would use them to simulate concurrent user behavior, identify breaking points, and generate performance benchmarks for your MCP implementations. The system is built as a modular engine that separates transport-aware clients, pattern-based session orchestration, and real-time metrics analysis into distinct, composable packages.

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
| `@reaatech/mcp-load-test-analysis` | published v0.1.0 | Analyzes Model Context Protocol (MCP) load test metrics to detect performance breaking points and assign letter grades based on latency, error rates, and recovery times. It prov… |
| `@reaatech/mcp-load-test-cli` | published v0.1.0 | Executes load, ramp, soak, and spike tests against Model Context Protocol (MCP) servers via a command-line interface. It generates performance reports in console, JSON, or Markd… |
| `@reaatech/mcp-load-test-client` | published v0.1.0 | Provides a unified client interface for Model Context Protocol (MCP) servers that automatically negotiates between stdio, SSE, and StreamableHTTP transports. It returns a sessio… |
| `@reaatech/mcp-load-test-core` | published v0.1.0 | Provides shared TypeScript types, Zod validation schemas, and utility functions for defining and analyzing MCP load test configurations. It serves as the foundational library fo… |
| `@reaatech/mcp-load-test-engine` | published v0.1.0 | Orchestrates load testing for Model Context Protocol (MCP) servers by managing session pools, concurrency patterns, and breaking point detection. It provides a `LoadEngine` clas… |
| `@reaatech/mcp-load-test-metrics` | published v0.1.0 | Aggregates latency histograms, error rates, and throughput metrics for Model Context Protocol (MCP) load testing. It provides a `MetricsCollector` class that processes individua… |
| `@reaatech/mcp-load-test-patterns` | published v0.1.0 | Provides a `PatternExecutor` class to run stateful, multi-step MCP tool-call sequences for load testing. It requires an MCP client and a metrics collector to execute predefined… |
| `@reaatech/mcp-load-test-profiles` | published v0.1.0 | Provides async generators that yield concurrency and phase metadata at one-second intervals to drive load testing session pools. It offers pre-built ramp, soak, spike, and custo… |
| `@reaatech/mcp-load-test-reporters` | published v0.1.0 | Provides classes to format MCP load test results into ANSI-colored console output, GitHub-flavored markdown, or machine-readable JSON. These reporters consume `LoadTestReport` o… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-load-test`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-load-test
- Browse packages: https://reaatech.com/products/testing-security/mcp-load-test/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, cli, developer-tools, mcp, mcp-server, mcp-tools, model-context-protocol, testing, typescript
