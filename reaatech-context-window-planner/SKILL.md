---
name: reaatech-context-window-planner
description: "These packages give you a deterministic engine for deciding what content to include, summarize, or drop when packing prompts into an LLM's context window. You'd adopt them to solve the problem of overflowing a model's token budget—replac…"
license: MIT
---

# REAA context-window-planner

These packages give you a deterministic engine for deciding what content to include, summarize, or drop when packing prompts into an LLM's context window. You'd adopt them to solve the problem of overflowing a model's token budget—replacing ad-hoc truncation with a configurable planner that enforces budgets, reserves space for generation, and emits structured warnings about every decision. The most distinctive thing is the pluggable strategy system: you can swap between priority-greedy, sliding-window, summarize-and-replace, or RAG relevance selection strategies, or compose custom ones, all while using typed context item primitives and tokenizer adapters for different model families.

## When to use this

Reach for `@reaatech/context-window-planner` when an agentic workflow must stay under a token budget while preserving essential data. It is the right fit when the user explicitly mentions “token budget”, “prompt too long”, “context overflow”, or “need to pack data by priority”. This family solves deterministic packing: it decides what to keep, summarize, or drop based on a configurable strategy and actual token counts (via `js-tiktoken`). Use it when you need a repeatable, testable plan rather than a heuristic truncation.

Concrete trigger phrases that map to this family:
- “The prompt exceeds the model’s context limit.”
- “Include high-priority items and summarize the rest.”
- “Drop items over 500 tokens.”
- “Build a packing plan that respects a token budget of 4000.”

The library is framework-agnostic: it works standalone, in a CLI batch pipeline, or embedded in a real-time agent loop. The builder pattern lets compose custom tokenizer adapters and packing strategies without coupling to a specific LLM provider.

## Quick start example

```typescript
import { ContextWindowPlanner, TokenizerAdapter } from '@reaatech/context-window-planner';
import { TiktokenAdapter } from '@reaatech/context-window-planner/tokenizers';

const tokenizer: TokenizerAdapter = new TiktokenAdapter('gpt-4');
const planner = new ContextWindowPlanner({ tokenizer, budget: 4096 });

planner.add({ id: 'instruction', content: 'Do X', priority: 'high' });
planner.add({ id: 'long_history', content: longConversation, priority: 'low' });

const result = planner.plan('priority-then-summarize');
console.log(result.included);   // items kept verbatim
console.log(result.summarized); // items replaced with summaries
console.log(result.dropped);    // items omitted
```

## Don’t reach for this when

- **When the budget is per-message sliding window** – use the native context management of the LLM SDK (e.g., `@anthropic-ai/sdk`’s sliding window mode) instead of a static plan.
- **When you need semantic retrieval to select relevant items** – this family does not rank by relevance. Use `@reaatech/retrieval-pipeline` or a third-party RAG library for embedding-based ranking before planning.
- **When token counting must be model‑specific with unknown or custom tokenizers** – the built-in adapter only supports `js-tiktoken` models. For a non‑OpenAI/Anthropic model, write a custom `TokenizerAdapter`.
- **When the input is streaming and the total size is unknown upfront** – the planner requires a complete list of items. For real‑time truncation mid‑stream, use a streaming token counter and a simpler trim strategy.

## Packages

```bash
npm install @reaatech/context-window-planner @reaatech/context-window-planner-cli
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/context-window-planner` | published v0.1.0 | A TypeScript library providing typed context items, packing strategies, and tokenizer adapters for deciding what to include, summarize, or drop when fitting prompts into LLM con… |
| `@reaatech/context-window-planner-cli` | published v0.1.0 | A CLI that reads a JSON context-window planning request from stdin and writes a packing plan (included, summarized, and dropped items with token counts) to stdout. |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-context-window-planner`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/context-window-planner
- Browse packages: https://reaatech.com/products/evals-quality/context-window-planner/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, developer-tools, llm, openai, prompt-engineering, rag, typescript
