---
name: reaatech-agents-md-kit
description: "These packages give you a linter, validator, and scaffolder for AGENTS.md and SKILL.md files — the markdown documents that define how AI agents behave and what skills they have. You would adopt them to enforce a consistent, machine-reada…"
license: MIT
---

# REAA agents-md-kit

These packages give you a linter, validator, and scaffolder for AGENTS.md and SKILL.md files — the markdown documents that define how AI agents behave and what skills they have. You would adopt them to enforce a consistent, machine-readable structure across agent definitions in a multi-agent system, catching formatting errors, missing sections, and broken skill references before they cause runtime issues. The toolkit is built as a pipeline of independent packages (parser → validator → linter → reporter) that share canonical Zod schemas, with an MCP server exposing the same tools directly to AI agents over Stdio or HTTP.

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
| `@reaatech/agents-markdown` | published v1.0.0 | Core domain types (`AgentsMdDocument`, `SkillMdDocument`, `Finding`, `LintResult`, `ValidationResult`), Zod validation schemas, and shared utilities for the `@reaatech/agents-ma… |
| `@reaatech/agents-markdown-cli` | published v1.0.0 | A CLI for linting, validating, scaffolding, and formatting AGENTS.md and SKILL.md files, providing five subcommands (`lint`, `validate`, `scaffold`, `format`, `examples`) with m… |
| `@reaatech/agents-markdown-linter` | published v1.0.0 | A linting engine for AGENTS.md and SKILL.md files that provides 18 built-in rules across style, content, and best-practice categories, exposed as `runLintRules`, `registerRule`,… |
| `@reaatech/agents-markdown-mcp-server` | published v1.0.0 | An MCP server that exposes five tools—`lint_agents_md`, `validate_agents_md`, `validate_skill_md`, `scaffold_agent`, and `get_examples`—for working with agent markdown files via… |
| `@reaatech/agents-markdown-observability` | published v1.0.0 | Pino-based structured logging, OpenTelemetry metrics and tracing, and dashboard aggregation for the `@reaatech/agents-markdown-*` ecosystem, exposed as functions (`info`, `error… |
| `@reaatech/agents-markdown-parser` | published v1.0.0 | Parses AGENTS.md and SKILL.md markdown files into typed documents with YAML frontmatter extraction, section hierarchy, table parsing, and code block extraction. Exports async fu… |
| `@reaatech/agents-markdown-reporter` | published v1.0.0 | Exports a set of reporter functions (`reportLintResult`, `reportJsonLintResult`, `reportHtmlLintResult`, `reportMarkdownLintResult`, and their validation counterparts) that form… |
| `@reaatech/agents-markdown-scaffold` | published v1.0.0 | A function that generates AGENTS.md and SKILL.md files from Handlebars templates, creating a complete agent directory structure with one skill file per listed skill ID. |
| `@reaatech/agents-markdown-validator` | published v1.0.0 | A Zod-based validation engine for AGENTS.md and SKILL.md files that checks frontmatter structure, required sections, section ordering, skill references, MCP tools tables, and co… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agents-md-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agents-md-kit
- Browse packages: https://reaatech.com/products/evals-quality/agents-md-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, agents-md, ai-agents, claude-code, cli, coding-agents, cursor, developer-tools, linter, opencode, scaffold, schema-validation, skills-md, typescript, validator, windsurf, instruction-artifacts, codex
