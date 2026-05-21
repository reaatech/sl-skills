---
name: reaatech-structured-output-repair
description: "These packages provide a pipeline to automatically fix malformed LLM outputs, such as invalid JSON, markdown fences, or type mismatches, to ensure they conform to your defined schemas. You would adopt them to prevent agent system crashes…"
license: MIT
---

# REAA structured-output-repair

These packages provide a pipeline to automatically fix malformed LLM outputs, such as invalid JSON, markdown fences, or type mismatches, to ensure they conform to your defined schemas. You would adopt them to prevent agent system crashes caused by unreliable model responses. The system uses a graduated, multi-strategy repair engine that can be integrated directly into your TypeScript code via `@reaatech/structured-repair-core` or exposed as an MCP server for use in tools like Claude Desktop.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Reliability & Ops** category. 2 packages live under `@reaatech/structured-repair-core` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/structured-repair-core` | pending npm | Fixes malformed LLM JSON output by applying a configurable pipeline of repair strategies to ensure compatibility with a provided Zod schema. It exports utility functions that re… |
| `@reaatech/structured-repair-mcp` | pending npm | Exposes tools for repairing malformed LLM outputs against JSON schemas via the Model Context Protocol. It provides an MCP server that can be run as a standalone process or integ… |

## Quick start

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-structured-output-repair`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/structured-output-repair
- Browse packages: https://reaatech.com/products/reliability-ops/structured-output-repair/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, developer-tools, json, llm, openai, typescript
