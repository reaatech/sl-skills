---
name: reaatech-agents-md-kit
description: "These packages provide a toolkit for managing `AGENTS.md` and `SKILL.md` files, which define the behavior and capabilities of AI agents. You would use them to automate the scaffolding, linting, and schema validation of these instruction…"
license: MIT
---

# REAA agents-md-kit

These packages provide a toolkit for managing `AGENTS.md` and `SKILL.md` files, which define the behavior and capabilities of AI agents. You would use them to automate the scaffolding, linting, and schema validation of these instruction artifacts within your development or CI/CD pipelines. The collection is built around a shared set of Zod schemas and domain types, ensuring consistent data structures across the parser, linter, and MCP server components.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Evals & Quality** category. 9 packages live under `@reaatech/agents-markdown` and siblings.

## Quick start example

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Packages

```bash
npm install @reaatech/agents-markdown @reaatech/agents-markdown-cli @reaatech/agents-markdown-linter @reaatech/agents-markdown-mcp-server @reaatech/agents-markdown-observability @reaatech/agents-markdown-parser @reaatech/agents-markdown-reporter @reaatech/agents-markdown-scaffold @reaatech/agents-markdown-validator
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agents-markdown` | published v1.0.0 | Provides shared TypeScript interfaces, Zod validation schemas, and utility functions for the AGENTS.md ecosystem. It serves as the central type definition library for document s… |
| `@reaatech/agents-markdown-cli` | published v1.0.0 | Provides a CLI and a set of utility functions for linting, validating, scaffolding, and formatting `AGENTS.md` and `SKILL.md` files. It supports recursive directory traversal, m… |
| `@reaatech/agents-markdown-linter` | published v1.0.0 | Enforces style, content, and best-practice standards for AGENTS.md and SKILL.md files through a set of exported functions for linting and auto-fixing. It requires a document obj… |
| `@reaatech/agents-markdown-mcp-server` | published v1.0.0 | Provides an MCP server that exposes tools for linting, validating, and scaffolding agent-related markdown files. It exports a factory function to create a configured MCP `Server… |
| `@reaatech/agents-markdown-observability` | published v1.0.0 | Provides structured Pino logging, OpenTelemetry metrics, and dashboard generation utilities for the `@reaatech/agents-markdown-*` ecosystem. It exports a collection of logging f… |
| `@reaatech/agents-markdown-parser` | published v1.0.0 | A Markdown AST parser that produces typed `Agents |
| `@reaatech/agents-markdown-reporter` | published v1.0.0 | Output reporters for lint and validation results that produce console, JSON, HTML, and GitHub-flavored Markdown reports. Exports a set of functions (`reportLintResult`, `reportJ… |
| `@reaatech/agents-markdown-scaffold` | published v1.0.0 | Generates `AGENTS.md` and `SKILL.md` documentation files for agent projects using Handlebars templates. It provides functions to scaffold directories based on a configuration ob… |
| `@reaatech/agents-markdown-validator` | published v1.0.0 | Validates AGENTS.md and SKILL.md files against the AGENTS.md specification, providing a set of functions (`validate`, `validateAgentsMd`, `validateSkillMd`) that accept parsed d… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agents-md-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agents-md-kit
- Browse packages: https://reaatech.com/products/evals-quality/agents-md-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, agents-md, ai-agents, claude-code, cli, coding-agents, cursor, developer-tools, linter, opencode, scaffold, schema-validation, skills-md, typescript, validator, windsurf, instruction-artifacts, codex
