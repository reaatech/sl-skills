---
name: reaatech-session-continuity-kit
description: "These packages provide a framework for managing multi-turn AI agent conversations, including token budget enforcement, context compression, and session persistence. They solve the complexity of maintaining consistent conversation state a…"
license: MIT
---

# REAA session-continuity-kit

These packages provide a framework for managing multi-turn AI agent conversations, including token budget enforcement, context compression, and session persistence. They solve the complexity of maintaining consistent conversation state across agent handoffs and varying LLM context windows. The system is built around a pluggable architecture where a central `SessionManager` coordinates with interchangeable storage adapters and model-specific tokenizers to handle session lifecycles.

## When to use this

Reach for `session-continuity-kit` when your agent needs to manage **conversation state across multiple turns**, enforce **token budget limits**, or **compress context windows** to stay within an LLM's maximum input length. This family solves the problem of maintaining a coherent, persistent session history that can survive agent handoffs, process restarts, or re-hydration from external storage.

Trigger on scenarios where the user mentions "multi-turn conversation", "keep context across interactions", "token budget", "session persistence", or "agent handoff". Also reach for it when the prompt contains phrases like "remember what we discussed earlier", "continue from where we left off", or "save conversation state". This family is the right choice when you need a pluggable storage backend (DynamoDB, Firestore, Redis, in-memory) and precise token counting for OpenAI or Anthropic models.

## Quick start example

The example below creates a `SessionManager` backed by an in-memory storage adapter with a token counter that uses a heuristic (character count / 4) as a simple fallback. In production, replace the counter with `@reaatech/session-continuity-tokenizers` and swap `MemoryAdapter` for a persistent adapter.

```typescript
import { SessionManager } from '@reaatech/session-continuity';
import { MemoryAdapter } from '@reaatech/session-continuity-storage-memory';
import { OpenAITokenCounter } from '@reaatech/session-continuity-tokenizers';

const adapter = new MemoryAdapter({ ttlMs: 3_600_000 }); // 1 hour TTL
const tokenCounter = new OpenAITokenCounter({ model: 'gpt-4' });
const manager = new SessionManager({ adapter, tokenCounter, maxTokens: 4096 });

await manager.createSession('session-1', { userId: 'abc' });
await manager.addMessage('session-1', { role: 'user', content: 'Hello' });
const history = await manager.getHistory('session-1');
console.log(history.messages);
```

## Don't reach for this when

- **Single-turn, stateless requests.** If each call is independent and you don't need to persist or reference prior turns, use a direct API call (e.g., OpenAI SDK) without a session manager.
- **Simple conversation without persistence or budget enforcement.** If you only need to keep messages in memory for a few turns and don't care about token limits, a plain array suffices.
- **Need for retrieval-augmented generation (RAG) with external knowledge.** This family does not index or retrieve documents. Use `@reaatech/rag` for combining conversation context with retrieved chunks.
- **Real-time streaming of responses.** `session-continuity-kit` handles message storage, not LLM response streaming. Combine it with `@reaatech/streaming` for live output.
- **Collaborative, multi-user sessions with conflict resolution.** This family is single-writer per session. For multi-user edits or live collaboration, consider `@reaatech/realtime` or a CRDT-based approach.

## Packages

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/session-continuity` | pending npm | Manages multi-turn AI conversation state, token budgets, and context compression through a `SessionManager` class. It requires an implementation of `IStorageAdapter` and a `Toke… |
| `@reaatech/session-continuity-storage-dynamodb` | pending npm | Provides a DynamoDB storage adapter for the `@reaatech/session-continuity` library, implementing the `IStorageAdapter` interface for session and message persistence. It requires… |
| `@reaatech/session-continuity-storage-firestore` | pending npm | `FirestoreAdapter` is a class implementing `I |
| `@reaatech/session-continuity-storage-memory` | pending npm | Provides an in-memory `IStorageAdapter` implementation for the `@reaatech/session-continuity` library. It exposes a `MemoryAdapter` class that uses a `Map` to store session data… |
| `@reaatech/session-continuity-storage-redis` | pending npm | A Redis storage adapter implementing the ` |
| `@reaatech/session-continuity-tokenizers` | pending npm | Provides classes for calculating exact or heuristic token counts for OpenAI and Anthropic models, implementing the `TokenCounter` interface from `@reaatech/session-continuity`.… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-session-continuity-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/session-continuity-kit
- Browse packages: https://reaatech.com/products/domain-pipelines/session-continuity-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, llm, mcp, rag, typescript
