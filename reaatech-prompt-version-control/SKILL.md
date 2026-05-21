---
name: reaatech-prompt-version-control
description: "These packages provide a centralized system for managing the lifecycle of AI prompts, including version tracking, A/B testing, and evaluation-gated promotions. You would adopt them to move prompt management out of application code and in…"
license: MIT
---

# REAA prompt-version-control

These packages provide a centralized system for managing the lifecycle of AI prompts, including version tracking, A/B testing, and evaluation-gated promotions. You would adopt them to move prompt management out of application code and into a structured environment that supports staging, production tagging, and runtime retrieval. The system is designed as a unified ecosystem where a Hono-based API server acts as the source of truth, accessible via a TypeScript SDK, a CLI, and an MCP server for direct integration with AI agents.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Evals & Quality** category. 5 packages live under `@reaatech/prompt-version-control` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/prompt-version-control` | pending npm | Provides a typed client class for interacting with the Prompt Version Control API, featuring built-in exponential backoff retries, request timeouts, and optional in-memory respo… |
| `@reaatech/prompt-version-control-cli` | pending npm | Manages prompt templates, versioning, and lifecycle tagging via a CLI that interacts with a remote prompt-version-control API. It provides a suite of commands for CRUD operation… |
| `@reaatech/prompt-version-control-mcp` | pending npm | Exposes a Model Context Protocol server that provides a `prompt.get` tool for fetching and rendering version-controlled prompts via Handlebars. It requires a configured Prompt V… |
| `@reaatech/prompt-version-control-server` | pending npm | A Hono app that exposes a REST API for Git-like version control of AI prompts, with eval-gated staging-to-production promotion, A/B traffic splitting, metrics ingestion, and web… |
| `@reaatech/prompt-version-control-shared` | pending npm | Provides shared TypeScript types, Zod schemas, and utility functions for validating, checksumming, and rendering prompt templates within the Prompt Version Control ecosystem. It… |

## Quick start

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-prompt-version-control`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/prompt-version-control
- Browse packages: https://reaatech.com/products/evals-quality/prompt-version-control/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, generative-ai, llm, mcp, mcp-server, prompt-engineering, typescript
