---
name: reaatech-mcp-load-test
description: "These packages provide a load testing framework designed to evaluate the performance and reliability of Model Context Protocol (MCP) servers. You would use them to simulate concurrent user behavior, identify breaking points, and generate…"
license: MIT
---

# REAA mcp-load-test

These packages provide a load testing framework designed to evaluate the performance and reliability of Model Context Protocol (MCP) servers. You would use them to simulate concurrent user behavior, identify breaking points, and generate performance benchmarks for your MCP implementations. The system is built as a modular engine that separates transport-aware clients, pattern-based session orchestration, and real-time metrics analysis into distinct, composable packages.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Testing & Security** category. 9 packages live under `@reaatech/mcp-load-test-analysis` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

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
