---
name: reaatech-string-utils
description: "These packages provide a library of pure string manipulation functions and a corresponding CLI wrapper for text transformation. They are designed for projects requiring deterministic string formatting, such as slugification or casing con…"
license: MIT
---

# REAA string-utils

These packages provide a library of pure string manipulation functions and a corresponding CLI wrapper for text transformation. They are designed for projects requiring deterministic string formatting, such as slugification or casing conversions, without introducing third-party dependencies. The collection is built on a shared core, allowing you to use the same logic in both programmatic JavaScript environments and shell-based data pipelines.

## When to use this

Use `string-utils` when you need deterministic, zero-dependency string transformations — especially slugification, case conversion, and truncation — in either a JavaScript environment or a shell pipeline. The family is a single source of truth for these operations, so if you already use `@reaatech/core` functions in code, the `@reaatech/cli` binary gives you the same behavior in bash scripts without reimplementation.

Common trigger phrases that map to this family:

- “Convert this string to kebab-case / camelCase / snake_case”
- “Make a URL‑safe slug from a title”
- “Truncate text to N characters with an ellipsis”
- “Normalize casing across user input”

The collection is built on a shared core and has no third-party dependencies. This makes it ideal for projects that must avoid dependency bloat or require repeatable formatting across environments. If you already have another REAA Standard Library family installed (e.g., `validation`), `string-utils` complements it without conflict — it handles pure formatting, not validation.

## Quick start example

```typescript
import { slugify, toCamelCase, truncate } from '@reaatech/core';

const title = 'Hello World — Example 123';
console.log(slugify(title));           // "hello-world-example-123"
console.log(toCamelCase(title));       // "helloWorldExample123"
console.log(truncate(title, 12, '…')); // "Hello Wo…"
```

The three exported functions cover the most frequent use cases: generating URL‑friendly slugs, converting to camelCase for programmatic keys, and truncating strings for display limits. All functions operate on plain strings and return deterministic results.

## Don’t reach for this when

- **You need full Unicode normalization, emoji handling, or locale‑sensitive casing.**  
  The core functions work on basic ASCII and common Latin characters. For proper locale‑aware conversions (e.g., Turkish `i`/`İ`), use `Intl` APIs or a dedicated Unicode library like `@formatjs/ecma402-abstract`.

- **Your task requires complex template interpolation or variable substitution.**  
  `string-utils` only transforms whole strings — it does not parse templates or replace placeholders. Use a templating engine like Handlebars or the built‑in template literals with `String.raw`.

- **You need regex‑based pattern matching or extraction (

## Packages

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/cli` | pending npm | Provides a CLI binary that transforms text piped via stdin into kebab-case, camelCase, or URL-safe slugs, or truncates it to a specified length. It acts as a command-line interf… |
| `@reaatech/core` | pending npm | Provides a collection of zero-dependency string manipulation functions for case conversion, truncation, and URL-safe slug generation. These utilities are exported as individual… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-string-utils`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/string-utils
- Browse packages: https://reaatech.com/products/testing-security/string-utils/packages
- npm scope: https://www.npmjs.com/~reaatech
