---
name: reaatech-agent-memory
description: "These packages give AI agents a long-term memory layer that persists information across sessions, not just within a single conversation. You'd adopt them to solve the problem of agents forgetting user preferences, contradicting themselve…"
license: MIT
---

# REAA agent-memory

These packages give AI agents a long-term memory layer that persists information across sessions, not just within a single conversation. You'd adopt them to solve the problem of agents forgetting user preferences, contradicting themselves, or accumulating irrelevant information over time. The most distinctive thing is that memory is treated as a managed asset with an explicit lifecycle—extraction, decay, forgetting, and contradiction resolution—rather than a fire-and-forget vector store.

## When to use this

Reach for agent-memory when your AI agent needs to remember facts, preferences, or decisions across multiple conversation turns or sessions, and you want automated lifecycle management — decay, contradiction resolution, and importance-based retention — rather than raw vector search. This family solves the problem of turning unstructured dialogue into structured, long-lived memory that the agent can query later with semantic, temporal, or priority filters.

Trigger phrases that signal a fit:
- “remember that I prefer dark mode”
- “you previously said you’d send the report”
- “user wants the agent to recall past decisions”
- “maintain a persistent knowledge base from conversations”

If the task involves “agent memory with forgetting rules,” “contradiction handling,” or “importance scoring,” this is the right family. The unified `AgentMemory` class, plus modular storage, embedding, LLM, and policy engines, let you compose exactly the memory behavior you need without reinventing the lifecycle logic.

## Quick start example

The following creates an agent memory system using in-memory storage and OpenAI providers, then extracts and stores a fact from a conversation turn and retrieves relevant memories by semantic query.

```typescript
import { AgentMemory } from '@reaatech/agent-memory';
import { InMemoryStorage } from '@reaatech/agent-memory-storage';
import { OpenAIEmbeddingProvider } from '@reaatech/agent-memory-embedding';
import { OpenAILLMProvider } from '@reaatech/agent-memory-llm';

const memory = new AgentMemory({
  storage: new InMemoryStorage(),
  embedding: new OpenAIEmbeddingProvider({ apiKey: process.env.OPENAI_API_KEY }),
  llm: new OpenAILLMProvider({ apiKey: process.env.OPENAI_API_KEY }),
});

await memory.extractAndStore("User said they love Italian food.");
const results = await memory.retrieveRelevant("food preferences");
console.log(results);
```

## Don't reach for this when

- You only need simple vector search without decay, contradiction resolution, or importance policies. Use a dedicated vector database or `@

## Packages

```bash
npm install @reaatech/agent-memory @reaatech/agent-memory-core @reaatech/agent-memory-embedding @reaatech/agent-memory-events @reaatech/agent-memory-extraction @reaatech/agent-memory-llm @reaatech/agent-memory-policies @reaatech/agent-memory-retrieval @reaatech/agent-memory-storage
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-memory` | published v0.1.0 | A class that provides a long-term memory layer for AI agents, combining LLM-powered extraction, semantic search, configurable storage (in-memory or PostgreSQL pgvector), and lif… |
| `@reaatech/agent-memory-core` | published v0.1.0 | Canonical TypeScript types, enums, and utilities for the agent-memory ecosystem, providing the `Memory` data structure, lifecycle states, importance levels, contradiction strate… |
| `@reaatech/agent-memory-embedding` | published v0.1.0 | An embedding provider abstraction that exposes a unified `EmbeddingProvider` interface with `embed()` and `embedBatch()` methods, shipping adapters for OpenAI, Cohere, and Huggi… |
| `@reaatech/agent-memory-events` | published v0.1.0 | An event bus and typed event types for agent-memory lifecycle hooks, providing `InMemoryEventBus` (a class with `on`, `off`, `once`, and `emit` methods) and nine event type cons… |
| `@reaatech/agent-memory-extraction` | published v0.1.0 | An LLM-powered memory extraction engine that analyzes conversation turns to identify and classify memorable facts, preferences, decisions, and corrections, returning structured… |
| `@reaatech/agent-memory-llm` | published v0.1.0 | An `LLMProvider` interface and `OpenAILLMProvider` class that expose `complete()` for freeform text generation and `completeStructured<T>()` for JSON-schema-constrained structur… |
| `@reaatech/agent-memory-policies` | published v0.1.0 | A policy engine for memory lifecycle management that provides decay scoring, forgetting decisions, and contradiction resolution, exposed as a `PolicyEngine` class that composes… |
| `@reaatech/agent-memory-retrieval` | published v0.1.0 | A semantic memory retriever for LLM agents that combines embedding similarity with recency, importance, and topic diversification, exposing a `MemoryRetriever` class with plugga… |
| `@reaatech/agent-memory-storage` | published v0.1.0 | A `MemoryStorage` interface (class) with 14 methods for CRUD, batch operations, similarity search, metadata filtering, health checks, and backup/restore of agent memories, plus… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-memory`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-memory
- Browse packages: https://reaatech.com/products/domain-pipelines/agent-memory/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, embeddings, generative-ai, llm, rag, semantic-search, typescript, vector-database
