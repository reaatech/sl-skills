---
name: reaatech-agent-memory
description: "These packages provide a managed long-term memory layer for AI agents, handling the extraction, storage, and retrieval of facts and preferences. You would adopt them to move beyond simple vector search by implementing active lifecycle ma…"
license: MIT
---

# REAA agent-memory

These packages provide a managed long-term memory layer for AI agents, handling the extraction, storage, and retrieval of facts and preferences. You would adopt them to move beyond simple vector search by implementing active lifecycle management, including automated decay, contradiction resolution, and importance-based retention. The system is built as a modular set of providers and policies, allowing you to swap storage backends like PostgreSQL or integrate custom retention rules while maintaining a unified interface for agent state.

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
| `@reaatech/agent-memory` | published v0.1.0 | Provides a unified interface for managing AI agent long-term memory, including automated fact extraction, semantic retrieval, and lifecycle policies. It exposes an `AgentMemory`… |
| `@reaatech/agent-memory-core` | published v0.1.0 | Provides the canonical TypeScript interfaces, enums, and utility functions for the agent-memory ecosystem, including vector similarity calculations and retry logic. It serves as… |
| `@reaatech/agent-memory-embedding` | published v0.1.0 | Provides a unified interface for generating text embeddings using OpenAI, Cohere, or HuggingFace providers. It includes a decorator class for transparently caching results in me… |
| `@reaatech/agent-memory-events` | published v0.1.0 | Provides an in-memory event bus and a set of TypeScript interfaces for hooking into agent memory lifecycle events like storage, retrieval, and contradiction resolution. It expos… |
| `@reaatech/agent-memory-extraction` | published v0.1.0 | Extracts structured facts, preferences, and decisions from conversation logs using LLMs and generates corresponding vector embeddings. It provides a `MemoryExtractor` class that… |
| `@reaatech/agent-memory-llm` | published v0.1.0 | Provides a unified interface for LLM text completion and structured JSON output. It includes a pre-built class for OpenAI-compatible APIs and allows custom implementations via t… |
| `@reaatech/agent-memory-policies` | published v0.1.0 | Provides a `PolicyEngine` class to manage the lifecycle of agent memories through configurable rules for exponential decay, automated forgetting, and contradiction resolution. I… |
| `@reaatech/agent-memory-retrieval` | published v0.1.0 | Provides a `MemoryRetriever` class to query and rank stored memories using semantic, temporal, and importance-based strategies, alongside a `ContextInjector` to format results f… |
| `@reaatech/agent-memory-storage` | published v0.1.0 | Provides a unified interface for persisting and querying agent memories, offering both an in-memory implementation and a PostgreSQL adapter with pgvector support. It includes a… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-memory`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-memory
- Browse packages: https://reaatech.com/products/domain-pipelines/agent-memory/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, embeddings, generative-ai, llm, rag, semantic-search, typescript, vector-database
