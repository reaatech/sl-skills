---
name: reaatech-prompt-version-control
description: "These packages provide a centralized system for managing the lifecycle of AI prompts, including version tracking, A/B testing, and evaluation-gated promotions. You would adopt them to move prompt management out of application code and in…"
license: MIT
---

# REAA prompt-version-control

These packages provide a centralized system for managing the lifecycle of AI prompts, including version tracking, A/B testing, and evaluation-gated promotions. You would adopt them to move prompt management out of application code and into a structured environment that supports staging, production tagging, and runtime retrieval. The system is designed as a unified ecosystem where a Hono-based API server acts as the source of truth, accessible via a TypeScript SDK, a CLI, and an MCP server for direct integration with AI agents.

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
# (no packages published to npm yet — install from source or wait for publish)
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/prompt-version-control` | pending npm | Provides a typed client class for interacting with the Prompt Version Control API, featuring built-in exponential backoff retries, request timeouts, and optional in-memory respo… |
| `@reaatech/prompt-version-control-cli` | pending npm | Manages prompt templates, versioning, and lifecycle tagging via a CLI that interacts with a remote prompt-version-control API. It provides a suite of commands for CRUD operation… |
| `@reaatech/prompt-version-control-mcp` | pending npm | Exposes a Model Context Protocol server that provides a `prompt.get` tool for fetching and rendering version-controlled prompts via Handlebars. It requires a configured Prompt V… |
| `@reaatech/prompt-version-control-server` | pending npm | A Hono app that exposes a REST API for Git-like version control of AI prompts, with eval-gated staging-to-production promotion, A/B traffic splitting, metrics ingestion, and web… |
| `@reaatech/prompt-version-control-shared` | pending npm | Provides shared TypeScript types, Zod schemas, and utility functions for validating, checksumming, and rendering prompt templates within the Prompt Version Control ecosystem. It… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-prompt-version-control`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/prompt-version-control
- Browse packages: https://reaatech.com/products/evals-quality/prompt-version-control/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, generative-ai, llm, mcp, mcp-server, prompt-engineering, typescript
