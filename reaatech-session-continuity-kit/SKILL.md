---
name: reaatech-session-continuity-kit
description: "These packages give you a complete session management layer for multi-turn AI agent conversations, handling conversation history windowing, token budget enforcement, context compression, and agent handoff. You'd adopt them to avoid rebui…"
license: MIT
---

# REAA session-continuity-kit

These packages give you a complete session management layer for multi-turn AI agent conversations, handling conversation history windowing, token budget enforcement, context compression, and agent handoff. You'd adopt them to avoid rebuilding the same session lifecycle logic—create, update, end, delete, and list sessions with participants and messages—that every agent system needs. The most distinctive thing is the pluggable storage adapter interface with production backends for Firestore, DynamoDB, and Redis, each implementing optimistic concurrency with version-checked writes and deterministic message ordering, plus three compression strategies (sliding window, summarization, hybrid) with cached summaries so the summarizer isn't re-invoked on every fetch.

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
npm install @reaatech/session-continuity @reaatech/session-continuity-storage-dynamodb @reaatech/session-continuity-storage-firestore @reaatech/session-continuity-storage-memory @reaatech/session-continuity-storage-redis @reaatech/session-continuity-tokenizers
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/session-continuity` | published v0.1.0 | A typed session lifecycle manager for multi-turn AI conversations, providing `SessionManager` with token budget enforcement, configurable context compression (sliding window, su… |
| `@reaatech/session-continuity-storage-dynamodb` | published v0.1.0 | A DynamoDB storage adapter implementing `IStorageAdapter` from `@reaatech/session-continuity`, providing session and message persistence using a single-table design with composi… |
| `@reaatech/session-continuity-storage-firestore` | published v0.1.0 | A Firestore storage adapter implementing `IStorageAdapter` from `@reaatech/session-continuity`, providing session and message persistence in Firestore collections with TTL suppo… |
| `@reaatech/session-continuity-storage-memory` | published v0.1.0 | An in-memory storage adapter implementing `IStorageAdapter` from `@reaatech/session-continuity`, using `Map`-based storage with optional simulated TTL expiration for development… |
| `@reaatech/session-continuity-storage-redis` | published v0.1.0 | A Redis storage adapter implementing `IStorageAdapter` from `@reaatech/session-continuity`, providing session and message persistence using Redis hashes, sorted sets, and native… |
| `@reaatech/session-continuity-tokenizers` | published v0.1.0 | A set of token counter implementations (exact WASM-based tiktoken for OpenAI, exact Anthropic, and a fast heuristic estimator) that implement the `TokenCounter` interface from `… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-session-continuity-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/session-continuity-kit
- Browse packages: https://reaatech.com/products/reliability-ops/session-continuity-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, developer-tools, llm, mcp, rag, typescript
