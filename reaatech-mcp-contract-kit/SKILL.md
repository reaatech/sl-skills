---
name: reaatech-mcp-contract-kit
description: "These packages give you a CLI tool and programmatic API for testing Model Context Protocol (MCP) servers against the MCP specification. You'd adopt them to validate that an MCP server correctly implements protocol conformance, registry Y…"
license: MIT
---

# REAA mcp-contract-kit

These packages give you a CLI tool and programmatic API for testing Model Context Protocol (MCP) servers against the MCP specification. You'd adopt them to validate that an MCP server correctly implements protocol conformance, registry YAML schemas, routing contracts, security posture, and performance baselines before deploying it. The packages are designed as composable layers—core types, a client SDK, validators, reporters, and observability—that you can use individually or together through the CLI, with all validators sharing the same typed test report format.

## When to use this

Reach for the mcp-contract-kit family whenever you need to validate that a Model Context Protocol (MCP) server implementation conforms to the official specification, passes security checks, or meets performance baselines. This is the right tool when an agent task involves automating conformance testing in a CI/CD pipeline, verifying a server’s advertised capabilities (tools, resources, prompts) against actual behavior, or generating structured compliance reports.

Trigger phrases that map here:
- “Validate MCP server against the spec”
- “MCP contract test in CI”
- “Check MCP protocol compliance”
- “Generate MCP conformance report”

The family solves a single category of problem: automated, reproducible validation of MCP server implementations using a shared core of Zod schemas, typed JSON-RPC 2.0 clients, and composable validator suites. It replaces ad-hoc curl scripts and manual testing with a deterministic, reportable test harness.

## Quick start example

The example below creates an MCP client, runs the default protocol‑compliance suite, and writes a Markdown report.

```typescript
import { createClient } from '@reaatech/mcp-contract-client'
import { compliance } from '@reaatech/mcp-contract-validators'
import { formatMarkdown } from '@reaatech/mcp-contract-reporters'

const client = createClient('http://localhost:8080/mcp')
const report = await compliance.test(client)
const markdown = formatMarkdown(report)
console.log(markdown)
```

The `compliance` suite tests tool discovery, request/response structure, and error handling. Replace `'http://localhost:8080/mcp'` with your server’s endpoint.

## Don’t reach for this when

- **You only need to call one MCP tool and get a result.** Use `@reaatech/mcp-contract-client` alone, or a lightweight HTTP client like `fetch`. The full contract kit adds test orchestration overhead you don’t need.
- **You must simulate a non‑compliant server or inject faults.** This family tests *against* a server; it does not stub or mock one. Use a server‑mocking library (e.g., `nock` for Node.js) or a dedicated MCP mock server.
- **You are testing an API that is not MCP.** The entire suite is built on MCP JSON‑RPC 2.0 types, Zod schemas, and the MCP lifecycle. For REST, GraphQL, or gRPC validation, reach for corresponding contract‑testing tools (e.g., `@reaatech/api-contract-kit` if applicable, or `Pact`/`Supertest`).
- **Load and performance tests need custom metrics without conformance checks.** Use purpose‑built load‑testing frameworks (k6, Artillery, `autocannon`) and separate observability tooling. The observability package here is designed for test harness audits, not production telemetry.

## Packages

```bash
npm install @reaatech/mcp-contract-cli @reaatech/mcp-contract-client @reaatech/mcp-contract-core @reaatech/mcp-contract-observability @reaatech/mcp-contract-reporters @reaatech/mcp-contract-validators
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-contract-cli` | published v0.1.0 | CLI tool and programmatic API for running conformance tests against MCP servers, covering protocol, registry, routing, security, and performance validators. Exports `runTests`,… |
| `@reaatech/mcp-contract-client` | published v0.1.0 | A factory function (`createMCPClient`) and class (`MCPHttpClient`) for connecting to Model Context Protocol servers over HTTP, providing tool discovery, tool invocation, JSON-RP… |
| `@reaatech/mcp-contract-core` | published v0.1.0 | Core domain types, JSON-RPC 2.0 schemas, and shared utilities for MCP contract validation, providing enums (`Severity`, `TestCategory`, `TestSuite`), result interfaces (`TestRes… |
| `@reaatech/mcp-contract-observability` | published v0.1.0 | A pino-based structured logger, in-memory metrics collector, and W3C trace context propagator for MCP contract validation. Exports a logger singleton with automatic PII redactio… |
| `@reaatech/mcp-contract-reporters` | published v0.1.0 | A set of reporter functions that consume `TestReport` objects from `@reaatech/mcp-contract-core` and render them as colored console output, JSON, GitHub-flavored Markdown, or a… |
| `@reaatech/mcp-contract-validators` | published v0.1.0 | A set of conformance validators for MCP servers that checks protocol compliance (JSON-RPC 2.0), registry configuration, routing contracts, security posture, and performance base… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-contract-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-contract-kit
- Browse packages: https://reaatech.com/products/testing-security/mcp-contract-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, conformance-testing, contract-testing, developer-tools, mcp, mcp-server, model-context-protocol, protocol-validation, spec-compliance, test-suite, testing-tools, typescript
