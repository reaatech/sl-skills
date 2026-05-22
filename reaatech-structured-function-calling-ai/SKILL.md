---
name: reaatech-structured-function-calling-ai
description: "These packages provide a unified framework for defining, validating, and executing LLM-compatible tools using Zod schemas. They allow you to standardize tool definitions once and deploy them across multiple LLM providers or MCP-compliant…"
license: MIT
---

# REAA structured-function-calling-ai

These packages provide a unified framework for defining, validating, and executing LLM-compatible tools using Zod schemas. They allow you to standardize tool definitions once and deploy them across multiple LLM providers or MCP-compliant clients without rewriting your integration logic. The system centers on a provider-agnostic registry and a middleware-based execution engine that handles schema enforcement, retries, and telemetry consistently across all tool calls.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Orchestration & Protocols** category. 7 packages live under `@reaatech/structured-function-calling-ai-adapter-anthropic` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/structured-function-calling-ai-adapter-anthropic` | pending npm | Normalizes tool definitions and execution results between provider-agnostic formats and Anthropic's specific tool-use schema. It provides utility functions to map between these… |
| `@reaatech/structured-function-calling-ai-adapter-google` | pending npm | Normalizes tool definitions and function calls between provider-agnostic formats and Google Gemini's specific schema requirements. It provides utility functions to map tool decl… |
| `@reaatech/structured-function-calling-ai-adapter-openai` | pending npm | Normalizes tool definitions and execution results between a provider-agnostic format and OpenAI’s specific function-calling schema. It provides utility functions to parse JSON a… |
| `@reaatech/structured-function-calling-ai-cli` | pending npm | Scaffold, validate, and dry-run AI tool definitions via a CLI or a set of exported utility functions. It requires `@reaatech/structured-function-calling-ai-core` and `@reaatech/… |
| `@reaatech/structured-function-calling-ai-core` | pending npm | Defines type-safe AI tool interfaces using Zod schemas and provides a `ToolRegistry` class to manage their execution. It includes a utility function to convert these Zod schemas… |
| `@reaatech/structured-function-calling-ai-engine` | pending npm | Provides a `ToolExecutor` class that manages the lifecycle of AI tool calls, including Zod schema validation, retry logic, and circuit breaking via a composable middleware pipel… |
| `@reaatech/structured-function-calling-ai-mcp-server` | pending npm | Exposes a `ToolRegistry` as a Model Context Protocol (MCP) server, allowing MCP clients to discover and execute registered functions. It provides an `McpToolServer` class that w… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-structured-function-calling-ai`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/structured-function-calling-ai
- Browse packages: https://reaatech.com/products/orchestration-protocols/structured-function-calling-ai/packages
- npm scope: https://www.npmjs.com/~reaatech
