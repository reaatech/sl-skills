---
name: reaatech-mcp-schema-evolution
description: "These packages provide tools to diff, classify, and validate changes between Model Context Protocol (MCP) tool snapshots. They help teams prevent breaking changes in their tool definitions by automating schema comparison and enforcing ev…"
license: MIT
---

# REAA mcp-schema-evolution

These packages provide tools to diff, classify, and validate changes between Model Context Protocol (MCP) tool snapshots. They help teams prevent breaking changes in their tool definitions by automating schema comparison and enforcing evolution policies within CI pipelines. The collection is built around a core library that returns result objects instead of throwing errors, allowing for consistent integration across CLI commands, CI checks, and custom validation scripts.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **MCP Infrastructure** category. 3 packages live under `@reaatech/mcp-schema-evolution` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-schema-evolution` | published v0.0.0 | Compare two MCP `Tool[]` snapshots and receive a fully classified |
| `@reaatech/mcp-schema-evolution-ci` | published v0.0.0 | Validates Model Context Protocol (MCP) tool schemas by comparing snapshots between branches to detect breaking changes. It provides both a CLI for CI pipelines and a programmati… |
| `@reaatech/mcp-schema-evolution-cli` | published v0.0.0 | Compare Model Context Protocol (MCP) tool snapshots to detect breaking schema changes via a CLI. It outputs human-readable reports or machine-readable JSON and uses exit codes t… |

## Quick start

```bash
npm install @reaatech/mcp-schema-evolution @reaatech/mcp-schema-evolution-ci @reaatech/mcp-schema-evolution-cli
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-schema-evolution`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-schema-evolution
- Browse packages: https://reaatech.com/products/mcp-infrastructure/mcp-schema-evolution/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, cli, developer-tools, mcp, mcp-server, mcp-tools, model-context-protocol, typescript
