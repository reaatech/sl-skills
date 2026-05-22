---
name: reaatech-context-window-planner
description: "These packages provide a deterministic engine for managing LLM context windows by deciding which data to include, summarize, or drop based on token budgets. They solve the problem of prompt overflow in complex agentic workflows where inp…"
license: MIT
---

# REAA context-window-planner

These packages provide a deterministic engine for managing LLM context windows by deciding which data to include, summarize, or drop based on token budgets. They solve the problem of prompt overflow in complex agentic workflows where input data frequently exceeds model limits. The system uses a modular builder pattern that allows you to compose custom packing strategies, tokenizer adapters, and typed context primitives into a single, framework-agnostic execution plan.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Domain Pipelines** category. 2 packages live under `@reaatech/context-window-planner` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/context-window-planner @reaatech/context-window-planner-cli
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/context-window-planner` | published v0.1.0 | Manages LLM context windows by providing a builder class, packing strategies, and typed primitives for prioritizing, summarizing, or dropping content. It relies on `js-tiktoken`… |
| `@reaatech/context-window-planner-cli` | published v0.1.0 | Provides a command-line interface for the `@reaatech/context-window-planner` library, accepting JSON via `stdin` to calculate token-aware packing plans for LLM context windows.… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-context-window-planner`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/context-window-planner
- Browse packages: https://reaatech.com/products/domain-pipelines/context-window-planner/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, developer-tools, llm, openai, prompt-engineering, rag, typescript
