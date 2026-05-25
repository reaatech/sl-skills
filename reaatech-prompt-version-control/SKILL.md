---
name: reaatech-prompt-version-control
description: "These packages give you Git-like version control for AI prompts — an API server, TypeScript SDK, CLI, and MCP server that let you track prompt changes, gate promotions on eval results, and serve A/B deployments. You'd adopt them to solve…"
license: MIT
---

# REAA prompt-version-control

These packages give you Git-like version control for AI prompts — an API server, TypeScript SDK, CLI, and MCP server that let you track prompt changes, gate promotions on eval results, and serve A/B deployments. You'd adopt them to solve the problem of managing prompt iterations across development, staging, and production without manual copy-pasting or ad-hoc versioning. The most distinctive thing is that the entire lifecycle — from creating a draft to promoting it to production — is gated by evaluation harness results, with AI agents able to pull managed prompts at runtime via the MCP server.

## When to use this

Reach for the prompt-version-control family when the task involves moving prompt management out of ad‑hoc environment variables or hardcoded strings into a structured, versioned system with staging, production aliases, and eval‑gated promotions. This fits when a user says “we need version control for our prompts,” “set up A/B testing for different prompt variants,” or “promote this prompt from staging to production only after it passes eval thresholds.” It also matches when the prompt contains runtime variables that must be interpolated via Handlebars and fetched dynamically from a central API. The system is designed for teams that want Git‑like branching and tagging for prompts, combined with traffic splitting and webhook‑driven notifications.

This family solves the problem of prompt lifecycle management outside of application code. It provides a Hono‑based API server as the source of truth, a TypeScript client for runtime retrieval, a CLI for manual edits and lifecycle commands, and an MCP server so that AI agents can fetch prompts directly. You would adopt it when you need a single pane of glass for prompt versions, staging vs. production environments, and metrics‑backed promotion decisions.

Trigger phrases that should cause an agent to consider this family: “version control for prompts,” “eval‑gated promotion,” “A/B test prompt templates,” “prompt lifecycle management,” “central prompt registry.” If the user mentions any of these, or asks to put prompts in a database with revision history, this family is the right choice.

## Quick start example

The example below assumes you have a Prompt Version Control API server running and a prompt alias `my-prompt` with a `prod` tag. The client handles retries and caching.

```typescript
import { PromptVersionControlClient } from '@reaatech/prompt-version-control';

const client = new PromptVersionControlClient({
  baseUrl: 'https://pvctl.example.com',
  apiKey: process.env.PVCTL_API_KEY
});

// Fetch the latest production version of a prompt
const prompt = await client.getPrompt({
  alias: 'my-prompt',
  tag: 'prod'
});

console.log(prompt.template); // => "Hello, {{name}}!"
console.log(prompt.variables); // => ["name"]

```

The client automatically applies exponential backoff on rate limits and respects a 10‑second request timeout. Optional in‑memory caching can be enabled via the `cache` option for frequently accessed prompts.

## Don't reach for this when

- **When you need to store only a handful of static prompts with no versioning.** In that case, a JSON file or environment variables are simpler and avoid the overhead of an API server and database.
- **When you require real‑time collaborative editing of prompts (like Google Docs).** This system is designed for versioned, immutable snapshots promoted through environments, not live collaborative editing. Use a shared document service or a code‑hosted prompt file with PR review instead.
- **When the prompts are generated dynamically per request from user input (e.g., RAG with retrieved context).** This family manages static templates – not on‑the‑fly assembly. For dynamic generation, look at the `@reaatech/llm‑pipeline` (if available) or a dedicated templating engine.
- **When you already have a prompt management solution integrated into your LLM platform (e.g., LangSmith Hub, OpenAI Prompt Management).** This family is designed for self‑hosted, API‑first control. If your provider’s built‑in tool covers your use case, prefer that to reduce operational burden.
- **When you need a simple function call to retrieve a prompt from a local file during development

## Packages

```bash
npm install @reaatech/prompt-version-control @reaatech/prompt-version-control-cli @reaatech/prompt-version-control-mcp @reaatech/prompt-version-control-server @reaatech/prompt-version-control-shared
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/prompt-version-control` | published v0.1.0 | A typed TypeScript client class (`PromptVersionControlClient` or `PVCClient`) for the Prompt Version Control API, providing authenticated HTTP access with automatic exponential-… |
| `@reaatech/prompt-version-control-cli` | published v0.1.0 | A CLI tool (`pvc`) for managing prompt templates, their versions, and lifecycle tags (draft/staging/production) from the terminal, built on Clipanion 4 with persistent configura… |
| `@reaatech/prompt-version-control-mcp` | published v0.1.0 | An MCP server that exposes a single `prompt.get` tool, which fetches a production-tagged prompt from a Prompt Version Control API server and renders it with Handlebars template… |
| `@reaatech/prompt-version-control-server` | published v0.1.0 | A Hono 4 server that provides Git-like version control for AI prompts, including CRUD, SHA-256 checksummed versioning, tag-based lifecycle (draft/staging/production), eval-gated… |
| `@reaatech/prompt-version-control-shared` | published v0.1.0 | Canonical TypeScript types, Zod schemas, and shared utilities (template rendering, API key generation, checksumming) for the Prompt Version Control ecosystem. Exports 25+ Zod sc… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-prompt-version-control`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/prompt-version-control
- Browse packages: https://reaatech.com/products/evals-quality/prompt-version-control/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, generative-ai, llm, mcp, mcp-server, prompt-engineering, typescript
