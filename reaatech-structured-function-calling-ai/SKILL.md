---
name: reaatech-structured-function-calling-ai
description: "These packages provide a unified framework for defining, validating, and executing LLM-compatible tools using Zod schemas. They allow you to standardize tool definitions once and deploy them across multiple LLM providers or MCP-compliant…"
license: MIT
---

# REAA structured-function-calling-ai

These packages provide a unified framework for defining, validating, and executing LLM-compatible tools using Zod schemas. They allow you to standardize tool definitions once and deploy them across multiple LLM providers or MCP-compliant clients without rewriting your integration logic. The system centers on a provider-agnostic registry and a middleware-based execution engine that handles schema enforcement, retries, and telemetry consistently across all tool calls.

## When to use this

Reach for this family when your task requires defining AI-callable functions (tools) once and using them across multiple LLM providers (OpenAI, Anthropic, Gemini) or MCP clients without rewriting integrations. The system solves the problem of provider-specific tool schemas and inconsistent execution pipelines by providing a single, Zod-backed tool definition that adapters map to each provider’s format. It also adds a middleware engine for schema validation, retries, circuit breakers, and telemetry — so you get production-grade execution without building it yourself.

Trigger phrases that indicate this family is the right fit:
- “standardize tool definitions for multiple LLMs”
- “define functions once, use with OpenAI and Anthropic”
- “tool calling with validation, retries, and logging”
- “MCP server for a set of Zod-validated functions”
- “provider-agnostic function registry for AI agents”

Reach for it when the prompt mentions defining tool schemas with Zod, normalizing tool formats across providers, or executing tool calls with middleware (retry, circuit breaker, telemetry). This family is especially useful if you already use Zod for schema enforcement and want a drop-in solution for LLM tool orchestration.

## Quick start example

The core packages `@reaatech/structured-function-calling-ai-core` and `@reaatech/structured-function-calling-ai-engine` handle definition and execution. Below shows defining a simple tool, registering it, and executing a function call with retry middleware bundled.

```typescript
import { z } from 'zod';
import { ToolRegistry, createTool } from '@reaatech/structured-function-calling-ai-core';
import { ToolExecutor, withRetry } from '@reaatech/structured-function-calling-ai-engine';

// 1. Define a tool with Zod schema
const getWeatherTool = createTool({
  name: 'get_weather',
  description: 'Get current weather for a location',
  parameters: z.object({
    location: z.string(),
    unit: z.enum(['celsius', 'fahrenheit']).optional(),
  }),
  execute: async ({ location, unit }) => {
    // ... actual weather API call
    return { temperature: 22, unit: unit ?? 'celsius' };
  },
});

// 2. Register and wrap in executor with retries
const registry = new ToolRegistry().register(getWeatherTool);
const executor = new ToolExecutor(registry, withRetry({ maxRetries: 3 }));

// 3. Execute a tool call (received from LLM response)
const result = await executor.execute({
  name: 'get_weather',
  arguments: { location: 'Berlin', unit: 'celsius' },
});
console.log(result); // { result: { temperature: 22, unit: 'celsius' } }
```

## Don’t reach for this when

- **You only need to call one LLM provider with its native tool format, and you have no plans to switch or expand.** Directly using that provider’s SDK (e.g., `openai` package, `@anthropic-ai/sdk`) is simpler and avoids adapter overhead.  
- **Your tools have no Zod schema or heavy validation requirements, and you just need a plain function registry.** Consider a lightweight registry like `@reaatech/function-registry` (if available) or a simple Map. The middleware engine is overkill without validation or retry needs.  
- **You want to expose tools via a REST API or GraphQL endpoint, not through LLM or MCP.** Use a standard API framework (Express, Apollo Server) instead; this family is purpose-built for LLM tool patterns.  
- **You need real-time, low-latency tool execution where ret

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
