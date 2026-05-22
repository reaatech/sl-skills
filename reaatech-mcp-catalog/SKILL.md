---
name: reaatech-mcp-catalog
description: "These packages provide a centralized registry for discovering, monitoring, and managing access to Model Context Protocol (MCP) servers across an organization. They solve the problem of fragmented tool availability by aggregating server s…"
license: MIT
---

# REAA mcp-catalog

These packages provide a centralized registry for discovering, monitoring, and managing access to Model Context Protocol (MCP) servers across an organization. They solve the problem of fragmented tool availability by aggregating server schemas and health status into a single searchable interface. The system is designed as a self-referential MCP server, allowing LLM agents to query the catalog for new tools using the same protocol they use to execute them.

## When to use this

Reach for `@reaatech/mcp-catalog-shared` when an agent or system needs to **discover, register, or query MCP servers and their capabilities** in a centralized registry. The user asks to find available tools, locate servers with specific resources or functions, or enumerate all accessible MCP endpoints across an organization. This is the "service discovery" layer for MCP: you use it when the prompt contains phrases like "find me a tool that can search the web", "list all MCP servers available", "show resources that match a query", or "search for capabilities by description". The shared package provides the canonical TypeScript types and runtime Zod schemas so any consumer (client, server, audit pipeline) can validate and interoperate with the catalog's domain objects—`Server`, `Tool`, `Resource`, `Capability`, `AccessPolicy`, etc.

This family is the right choice for any code that needs to **serialize, deserialize, or validate** catalog entities. It solves the "fragmented tool availability" problem by giving you a single, type-safe vocabulary for describing what MCP servers offer. An agent building a discovery panel, an orchestration layer that dynamically routes tool calls, or a monitoring service that checks health statuses all benefit from the shared types and validation logic.

## Quick start example

```typescript
import { Server, Tool, CreateServerInput } from '@reaatech/mcp-catalog-shared';
import { z } from 'zod';

// Validate a tool object coming from an API response
const tool: Tool = {
  id: 'web-search',
  name: 'search',
  description: 'Perform web search',
  inputSchema: z.object({ query: z.string() }),
  serverId: 'web-search-server',
};

// Validate a server creation payload at runtime
const input = { name: 'Web Search', description: 'Searches the web', baseUrl: 'https://mcp-search.example.com' };
const parsed = CreateServerInput.parse(input);
// parsed is now a typed Server object
console.log(parsed.name); // 'Web Search'
```

## Don't reach for this when

- **When you need to actually call an MCP tool or resource.** This package provides only types and validation schemas, not a client. Use `@modelcontextprotocol/sdk` or a dedicated MCP client library instead.
- **When you need an in-memory registry that doesn't persist across restarts.** The catalog is meant to be backed by a database or API. For ephemeral tool lists inside a single agent, just use a plain array or Map.
- **When you need to implement a brand new MCP server.** The shared types describe the *catalog's* domain model (servers, tools, access policies), not the MCP protocol itself. For building an MCP server, use `@modelcontextprotocol/sdk` and your own schema definitions.
- **When you need to manage authentication or authorization logic.** The `AccessPolicy` and `ApiKey` types describe catalog-level access, not user authentication. Use an identity provider and a separate auth layer for that.

## Packages

```bash
npm install @reaatech/mcp-catalog-shared
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/mcp-catalog-shared` | published v1.0.0 | Shared TypeScript type definitions and Zod validation schemas for MCP Catalog domain entities (`Server`, `Capability`, `Tool`, `Resource`, `User`, `ApiKey`, `AccessPolicy`, `Hea… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-mcp-catalog`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/mcp-catalog
- Browse packages: https://reaatech.com/products/mcp-infrastructure/mcp-catalog/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, mcp, mcp-server, mcp-tools, model-context-protocol, service-discovery, typescript
