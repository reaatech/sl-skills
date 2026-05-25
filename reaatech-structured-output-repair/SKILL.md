---
name: reaatech-structured-output-repair
description: "These packages give you a repair engine that takes a Zod schema and malformed LLM output, then runs it through six graduated strategies—stripping markdown fences, extracting JSON from prose, fixing syntax errors, coercing types, fuzzy-ma…"
license: MIT
---

# REAA structured-output-repair

These packages give you a repair engine that takes a Zod schema and malformed LLM output, then runs it through six graduated strategies—stripping markdown fences, extracting JSON from prose, fixing syntax errors, coercing types, fuzzy-matching keys, and removing extra fields—to return valid, schema-conforming data instead of crashing. You'd adopt them to handle the common failure modes of LLM structured output generation: trailing commas, truncated streams, Python literals, hallucinated field names, and JSON buried in conversational wrappers. The most distinctive thing is the graduated pipeline approach—each strategy runs in sequence, the engine validates after each step, and it returns as soon as the data conforms, giving you detailed diagnostics including per-field errors and best-effort partial data when full repair fails.

## When to use this

Reach for `@reaatech/structured-repair-core` when your application depends on LLM-generated JSON that frequently arrives malformed — broken syntax, markdown fences around the JSON object, type mismatches against a known schema, or missing required fields. The typical trigger is user feedback like “the AI response is not valid JSON” or “expected a number but got a string”. Any task that requires a fallible LLM to produce structured output, and where a retry is too costly or slow, maps directly to this package family.

Use the MCP server (`@reaatech/structured-repair-mcp`) when you need to offload repair logic to a separate process or integrate it with tools like Claude Desktop. The trigger is a request to “expose a repair tool for the LLM’s output” or “fix JSON before passing it to downstream systems”. The family solves a specific reliability gap: making model responses robust without compromising on schema strictness.

## Quick start example

The core package applies a pipeline of repair strategies to a malformed string and validates it against a Zod schema.

```typescript
import { z } from 'zod';
import { repairJson } from '@reaatech/structured-repair-core';

const UserSchema = z.object({
  name: z.string(),
  age: z.number().int(),
});

const malformed = '```json\n{"name": "Alice", "age": "30"}\n```'; // age is string, wrapped in fences

const result = repairJson(malformed, UserSchema);
if (result.status === 'repaired') {
  // result.data is type-inferred as { name: string; age: number }
  console.log(result.data.age); // 30 (number)
} else if (result.status === 'failed') {
  console.error('Repair exhausted all strategies', result.errors);
}
```

The returned `result` also contains `metadata` with the number of strategies attempted and the final repair path taken.

## Don't reach for this when

- **When the LLM output is only used for display and doesn’t need a schema.** Use a simple JSON parser with try/catch instead; repair overhead is unnecessary.
- **When you want to retry the LLM call on failure rather than repair.** If the model is consistently producing malformed output, a retry with a more explicit prompt is often cheaper than a multi-strategy repair pipeline.
- **When you need to repair non-JSON formats (XML, YAML, plain text).** This family only targets JSON-like structures. For other formats, use a dedicated parser or a general string manipulation approach.
- **When you are calling a model that already guarantees structured output (e.g., OpenAI structured outputs mode).** In those cases validation via Zod is sufficient without a repair step

## Packages

```bash
npm install @reaatech/structured-repair-core @reaatech/structured-repair-mcp
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/structured-repair-core` | published v1.0.0 | A Zod schema–driven repair engine that applies six graduated strategies (prose extraction, truncation repair, type coercion, fuzzy key matching, extra field removal, and input a… |
| `@reaatech/structured-repair-mcp` | published v1.0.0 | An MCP server that exposes `structured.repair` and `structured.analyze` as tools, letting MCP-compatible clients (Claude Desktop, Cursor) repair malformed LLM structured outputs… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-structured-output-repair`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/structured-output-repair
- Browse packages: https://reaatech.com/products/reliability-ops/structured-output-repair/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai, ai-agents, anthropic, developer-tools, json, llm, openai, typescript
