---
name: reaatech-mcp-contract-kit
description: "These packages provide a comprehensive suite for testing Model Context Protocol (MCP) server implementations against the official specification. You would adopt them to automate conformance, security, and performance validation of your M…"
license: MIT
---

# REAA mcp-contract-kit

These packages provide a comprehensive suite for testing Model Context Protocol (MCP) server implementations against the official specification. You would adopt them to automate conformance, security, and performance validation of your MCP servers within CI/CD pipelines or local development workflows. The collection is built on a shared foundation of Zod schemas and core domain types, allowing you to compose custom validation logic, client interactions, and reporting formats into a unified testing harness.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Testing & Security** category. 6 packages live under `@reaatech/mcp-contract-cli` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/mcp-contract-cli @reaatech/mcp-contract-client @reaatech/mcp-contract-core @reaatech/mcp-contract-observability @reaatech/mcp-contract-reporters @reaatech/mcp-contract-validators
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-contract-cli` | published v0.1.0 | Validates Model Context Protocol (MCP) servers against protocol, security, and performance standards using a CLI tool and a programmatic API. It provides functions to execute te… |
| `@reaatech/mcp-contract-client` | published v0.1.0 | Connects to and interacts with Model Context Protocol (MCP) servers over HTTP using a client class or factory function. It provides typed methods for JSON-RPC 2.0 communication,… |
| `@reaatech/mcp-contract-core` | published v0.1.0 | Provides foundational TypeScript interfaces, Zod schemas, and JSON-RPC 2.0 types for validating and interacting with Model Context Protocol (MCP) contracts. It serves as the sha… |
| `@reaatech/mcp-contract-observability` | published v0.1.0 | Provides structured logging via Pino, in-memory metrics collection, and W3C trace context propagation for Model Context Protocol (MCP) contract validation. It exports a suite of… |
| `@reaatech/mcp-contract-reporters` | published v0.1.0 | Converts `TestReport` objects from `@reaatech/mcp-contract-core` into console, JSON, Markdown, or HTML strings. It provides a collection of formatting functions and a dispatcher… |
| `@reaatech/mcp-contract-validators` | published v0.1.0 | Validates Model Context Protocol (MCP) server compliance, security, and performance through a collection of validator functions and suites. It provides individual validator obje… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-contract-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-contract-kit
- Browse packages: https://reaatech.com/products/testing-security/mcp-contract-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, conformance-testing, contract-testing, developer-tools, mcp, mcp-server, model-context-protocol, protocol-validation, spec-compliance, test-suite, testing-tools, typescript
