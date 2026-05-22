---
name: reaatech-agents-md-kit
description: "These packages provide a toolkit for managing `AGENTS.md` and `SKILL.md` files, which define the behavior and capabilities of AI agents. You would use them to automate the scaffolding, linting, and schema validation of these instruction…"
license: MIT
---

# REAA agents-md-kit

These packages provide a toolkit for managing `AGENTS.md` and `SKILL.md` files, which define the behavior and capabilities of AI agents. You would use them to automate the scaffolding, linting, and schema validation of these instruction artifacts within your development or CI/CD pipelines. The collection is built around a shared set of Zod schemas and domain types, ensuring consistent data structures across the parser, linter, and MCP server components.

## When to use this

Reach for `agents-md-kit` when the task involves managing `AGENTS.md` or `SKILL.md` files — the instruction artifacts that define AI agent behavior. The family solves the entire lifecycle: scaffolding initial files, parsing them into typed ASTs, validating against the specification, enforcing style and content rules via linting, and generating human-readable or CI-friendly reports.

Trigger phrases that map to this family: *“create an AGENTS.md for my agent”*, *“lint the agent markdown files”*, *“validate that AGENTS.md follows the spec”*, *“scaffold a SKILL.md”*, or *“set up CI checks for agent instruction files”*. Any prompt asking to automate, check, or generate these specific files is a strong signal. Also use it when a code review or pipeline step requires checking that agent definitions are well-formed and consistent across a monorepo.

The family is built for deterministic, dev-tool use inside automated workflows. It expects structured input (file paths, parsed documents) and returns structured output (validation errors, lint results). The MCP server package additionally makes these capabilities available to AI coding tools that support the Model Context Protocol, enabling in-editor linting and scaffolding.

## Quick start example

The most common pattern is parsing an existing `AGENTS.md`, validating it against the spec, and reporting the results. The following example reads the file, parses it, runs validation, and prints any issues.

```typescript
import { readFile } from 'node:fs/promises';
import { parseAgentsMd } from '@reaatech/agents-markdown-parser';
import { validate } from '@reaatech/agents-markdown-validator';
import { reportJsonLintResult } from '@reaatech/agents-markdown-reporter';

const content = await readFile('AGENTS.md', 'utf-8');
const document = parseAgentsMd(content);

const validationResult = validate(document);
if (!validationResult.valid) {
  process.stdout.write(reportJsonLintResult([validationResult]));
  process.exit(1);
}
```

The same flow works for `SKILL.md` by calling `parseSkillMd` and `validateSkillMd` respectively. For a complete CI-check, pipe the output of the `agents-markdown-cli` binary instead.

## Don't reach for this when

- **You need to lint or parse generic markdown files (e.g. README.md, CHANGELOG.md, blog posts).** This family is hard-wired to the `AGENTS.md` and `SKILL.md` schema. Use [markdownlint](https://github.com/DavidAnson/markdownlint) or a [unified/remark](https://unifiedjs.com/) ecosystem parser for any

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
