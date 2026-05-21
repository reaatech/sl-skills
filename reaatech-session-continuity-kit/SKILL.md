---
name: reaatech-session-continuity-kit
description: "These packages provide a framework for managing multi-turn AI agent conversations, including token budget enforcement, context compression, and session persistence. They solve the complexity of maintaining consistent conversation state a…"
license: MIT
---

# REAA session-continuity-kit

These packages provide a framework for managing multi-turn AI agent conversations, including token budget enforcement, context compression, and session persistence. They solve the complexity of maintaining consistent conversation state across agent handoffs and varying LLM context windows. The system is built around a pluggable architecture where a central `SessionManager` coordinates with interchangeable storage adapters and model-specific tokenizers to handle session lifecycles.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Domain Pipelines** category. 6 packages live under `@reaatech/session-continuity` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/session-continuity` | pending npm | Manages multi-turn AI conversation state, token budgets, and context compression through a `SessionManager` class. It requires an implementation of `IStorageAdapter` and a `Toke… |
| `@reaatech/session-continuity-storage-dynamodb` | pending npm | Provides a DynamoDB storage adapter for the `@reaatech/session-continuity` library, implementing the `IStorageAdapter` interface for session and message persistence. It requires… |
| `@reaatech/session-continuity-storage-firestore` | pending npm | `FirestoreAdapter` is a class implementing `I |
| `@reaatech/session-continuity-storage-memory` | pending npm | Provides an in-memory `IStorageAdapter` implementation for the `@reaatech/session-continuity` library. It exposes a `MemoryAdapter` class that uses a `Map` to store session data… |
| `@reaatech/session-continuity-storage-redis` | pending npm | A Redis storage adapter implementing the ` |
| `@reaatech/session-continuity-tokenizers` | pending npm | Provides classes for calculating exact or heuristic token counts for OpenAI and Anthropic models, implementing the `TokenCounter` interface from `@reaatech/session-continuity`.… |

## Quick start

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-session-continuity-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/session-continuity-kit
- Browse packages: https://reaatech.com/products/domain-pipelines/session-continuity-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, llm, mcp, rag, typescript
