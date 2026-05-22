---
name: reaatech-mcp-schema-evolution
description: "These packages provide tools to diff, classify, and validate changes between Model Context Protocol (MCP) tool snapshots. They help teams prevent breaking changes in their tool definitions by automating schema comparison and enforcing ev…"
license: MIT
---

# REAA mcp-schema-evolution

These packages provide tools to diff, classify, and validate changes between Model Context Protocol (MCP) tool snapshots. They help teams prevent breaking changes in their tool definitions by automating schema comparison and enforcing evolution policies within CI pipelines. The collection is built around a core library that returns result objects instead of throwing errors, allowing for consistent integration across CLI commands, CI checks, and custom validation scripts.

## When to use this

Reach for the `mcp-schema-evolution` family when the task involves comparing two sets of Model Context Protocol (MCP) tool definitions and classifying the differences — especially to detect breaking changes. The core library returns structured result objects (never throws) so you can integrate the diff into CI pipelines, CLI commands, or custom validation scripts without try/catch noise. The CI and CLI packages wrap this logic into ready-to-use pipelines and human‑readable reports.

Trigger phrases: *“diff MCP tool schemas”*, *“detect breaking changes in MCP tools”*, *“validate MCP tool evolution in CI”*, *“compare MCP tool snapshots”*. If the user asks to verify that an updated MCP server doesn’t break existing clients, or to build a pre‑commit hook that checks tool definitions, this family is the right fit. It solves the category problem of **schema evolution enforcement for MCP tool definitions** — not generic JSON diffing or runtime data validation.

The library is schema‑aware: it knows the structure of MCP `Tool` objects and can classify additions, removals, and modifications by their backward‑compatibility impact. This makes it trivial to fail a build when someone renames a required parameter or removes a tool that clients depend on.

## Quick start example

The core package provides `diffTools` which takes two `Tool[]` snapshots and returns a fully classified diff.

```typescript
import { diffTools } from '@reaatech/mcp-schema-evolution';

const oldTools = [
  { name: 'getWeather', inputSchema: { type: 'object', properties: { city: { type: 'string' } }, required: ['city'] } }
];

const newTools = [
  { name: 'getWeather', inputSchema: { type: 'object', properties: { city: { type: 'string' }, unit: { type: 'string' } }, required: ['city'] } }
];

const result = diffTools({ from: oldTools, to: newTools });

console.log(result.breaking);    // false (adding an optional property is non-breaking)
console.log(result.additions);   // [{ name: 'getWeather', path: 'inputSchema.properties.unit' }]
```

## Don’t reach for this when

- You need to validate a JSON Schema definition against actual data — use a standard JSON Schema validator like `ajv`.
- You need to enforce runtime API contract compliance between services — this family only compares static MCP `Tool` definitions; use tools like `@reaatech/openapi-evolution` for OpenAPI contracts.
- You need to generate a new MCP tool implementation from scratch — use the official MCP SDK or a higher‑level framework; this family provides no code generation.
- You need a general‑purpose JSON diff (e.g. comparing two arbitrary database records) — use a library like `deep-diff` or `jsondiffpatch` that doesn’t understand MCP semantics.
- You need to monitor a live MCP server for schema changes at runtime — this family is designed for static snapshot comparison in CI, not runtime observation.

## Packages

```bash
npm install @reaatech/mcp-schema-evolution @reaatech/mcp-schema-evolution-ci @reaatech/mcp-schema-evolution-cli
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-schema-evolution` | published v0.0.0 | Compare two MCP `Tool[]` snapshots and receive a fully classified |
| `@reaatech/mcp-schema-evolution-ci` | published v0.0.0 | Validates Model Context Protocol (MCP) tool schemas by comparing snapshots between branches to detect breaking changes. It provides both a CLI for CI pipelines and a programmati… |
| `@reaatech/mcp-schema-evolution-cli` | published v0.0.0 | Compare Model Context Protocol (MCP) tool snapshots to detect breaking schema changes via a CLI. It outputs human-readable reports or machine-readable JSON and uses exit codes t… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-schema-evolution`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-schema-evolution
- Browse packages: https://reaatech.com/products/mcp-infrastructure/mcp-schema-evolution/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, cli, developer-tools, mcp, mcp-server, mcp-tools, model-context-protocol, typescript
