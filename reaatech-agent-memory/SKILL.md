---
name: reaatech-agent-memory
description: "These packages provide a managed long-term memory layer for AI agents, handling the extraction, storage, and retrieval of facts and preferences. You would adopt them to move beyond simple vector search by implementing active lifecycle ma…"
license: MIT
---

# REAA agent-memory

These packages provide a managed long-term memory layer for AI agents, handling the extraction, storage, and retrieval of facts and preferences. You would adopt them to move beyond simple vector search by implementing active lifecycle management, including automated decay, contradiction resolution, and importance-based retention. The system is built as a modular set of providers and policies, allowing you to swap storage backends like PostgreSQL or integrate custom retention rules while maintaining a unified interface for agent state.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Domain Pipelines** category. 9 packages live under `@reaatech/agent-memory` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-memory` | published v0.1.0 | Provides a unified interface for managing AI agent long-term memory, including automated fact extraction, semantic retrieval, and lifecycle policies. It exposes an `AgentMemory`… |
| `@reaatech/agent-memory-core` | published v0.1.0 | Provides the canonical TypeScript interfaces, enums, and utility functions for the agent-memory ecosystem, including vector similarity calculations and retry logic. It serves as… |
| `@reaatech/agent-memory-embedding` | published v0.1.0 | Provides a unified interface for generating text embeddings using OpenAI, Cohere, or HuggingFace providers. It includes a decorator class for transparently caching results in me… |
| `@reaatech/agent-memory-events` | published v0.1.0 | Provides an in-memory event bus and a set of TypeScript interfaces for hooking into agent memory lifecycle events like storage, retrieval, and contradiction resolution. It expos… |
| `@reaatech/agent-memory-extraction` | published v0.1.0 | Extracts structured facts, preferences, and decisions from conversation logs using LLMs and generates corresponding vector embeddings. It provides a `MemoryExtractor` class that… |
| `@reaatech/agent-memory-llm` | published v0.1.0 | Provides a unified interface for LLM text completion and structured JSON output. It includes a pre-built class for OpenAI-compatible APIs and allows custom implementations via t… |
| `@reaatech/agent-memory-policies` | published v0.1.0 | Provides a `PolicyEngine` class to manage the lifecycle of agent memories through configurable rules for exponential decay, automated forgetting, and contradiction resolution. I… |
| `@reaatech/agent-memory-retrieval` | published v0.1.0 | Provides a `MemoryRetriever` class to query and rank stored memories using semantic, temporal, and importance-based strategies, alongside a `ContextInjector` to format results f… |
| `@reaatech/agent-memory-storage` | published v0.1.0 | Provides a unified interface for persisting and querying agent memories, offering both an in-memory implementation and a PostgreSQL adapter with pgvector support. It includes a… |

## Quick start

```bash
npm install @reaatech/agent-memory @reaatech/agent-memory-core @reaatech/agent-memory-embedding @reaatech/agent-memory-events @reaatech/agent-memory-extraction @reaatech/agent-memory-llm @reaatech/agent-memory-policies @reaatech/agent-memory-retrieval @reaatech/agent-memory-storage
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-memory`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-memory
- Browse packages: https://reaatech.com/products/domain-pipelines/agent-memory/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, embeddings, generative-ai, llm, rag, semantic-search, typescript, vector-database
