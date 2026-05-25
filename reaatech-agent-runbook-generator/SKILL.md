---
name: reaatech-agent-runbook-generator
description: "These packages give you a CLI, library, and MCP server that scan a service repository and produce a complete operator runbook—alerts, dashboards, failure modes, rollback steps, incident workflows, health checks, and dependency maps. You…"
license: MIT
---

# REAA agent-runbook-generator

These packages give you a CLI, library, and MCP server that scan a service repository and produce a complete operator runbook—alerts, dashboards, failure modes, rollback steps, incident workflows, health checks, and dependency maps. You would adopt them to automate the creation and maintenance of runbooks for every service in your organization, replacing manual documentation that goes stale. The packages are designed as independent, composable modules (analyzer, alerts, dashboards, etc.) that share core types and Zod schemas, so you can use the full pipeline via the CLI or pick individual packages for programmatic use.

## When to use this

Reach for this family when your agent’s task involves generating or updating operator runbooks by analyzing a service repository. The core workflow is: scan a codebase to extract its language, framework, endpoints, and dependencies; derive alerts, dashboards, failure modes, health checks, rollback plans, and incident response workflows; then assemble the results into a structured, multi-format runbook.

Trigger phrases to watch for:  
- “generate an operator runbook from our repo”  
- “runbook is out of date — regenerate it from the code”  
- “create incident response docs for this service”  
- “automate our runbook maintenance”  
- any mention of needing “failure modes”, “health checks”, “rollback procedures”, or “service maps” derived from source code.

This family works best when the runbook’s content should be programmatically inferred from the repository, not hand-written or maintained separately. It replaces manual, static documentation with dynamically generated, always-current operational context.

## Quick start example

```typescript
import { scanRepository } from '@reaatech/agent-runbook-analyzer';
import { AnalysisAgent } from '@reaatech/agent-runbook-agent';
import { assembleRunbook } from '@reaatech/agent-runbook-runbook';

// 1. Scan the repository to extract metadata
const analysis = await scanRepository('/path/to/service');

// 2. Use an LLM agent to enrich and validate the analysis
const agent = new AnalysisAgent({ provider: 'anthropic' });
const enriched = await agent.analyze(analysis);

// 3. Generate the final runbook as Markdown and JSON
const runbook = await assembleRunbook(enriched, { format: ['md', 'json'] });
console.log(runbook.markdown);
```

## Don’t reach for this when

- **You only need a service dependency graph, not a full runbook.** Use [`@reaatech/agent-runbook-service-map`](https://github.com/reaatech/agent-runbook-service-map) directly to export a directed graph in Mermaid or DOT format.
- **You want to configure alerting rules manually in a monitoring tool.** Use the infrastructure-as-code tooling for that platform (e.g., Terraform’s Datadog provider) rather than generating alerts from code analysis.
- **Your runbook content is primarily human-written narrative with no direct code derivation.** This family adds overhead if you aren’t scanning a repository – write static docs with a tool like Docusaurus.
- **You need real-time runtime observability, not static runbook generation.** Use an APM or tracing setup (e.g., OpenTelemetry collector) directly; this family only produces documentation from static analysis.
- **Your environment is not Node.js or TypeScript.** The entire package family is built for the Node.js runtime; other environments cannot import the Zod schemas or run the scanner.

## Packages

```bash
npm install @reaatech/agent-runbook @reaatech/agent-runbook-agent @reaatech/agent-runbook-alerts @reaatech/agent-runbook-analyzer @reaatech/agent-runbook-cli @reaatech/agent-runbook-dashboards @reaatech/agent-runbook-failure-modes @reaatech/agent-runbook-health-checks @reaatech/agent-runbook-incident @reaatech/agent-runbook-mcp @reaatech/agent-runbook-observability @reaatech/agent-runbook-rollback @reaatech/agent-runbook-runbook @reaatech/agent-runbook-service-map
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/agent-runbook` | published v0.1.0 | Shared domain types, Zod schemas, utility functions, and error classes for the Agent Runbook Generator ecosystem. Exports TypeScript interfaces (e.g., `Runbook`, `ServiceDefinit… |
| `@reaatech/agent-runbook-agent` | published v0.1.0 | An AI agent class (`AnalysisAgent`) and factory function (`createAnalysisAgent`) that wraps Anthropic Claude, OpenAI, and Google Gemini LLMs with pre-built prompt templates for… |
| `@reaatech/agent-runbook-alerts` | published v0.1.0 | Extracts existing alert definitions from Prometheus, Datadog, and CloudWatch configs and generates new SLO-based, resource, and application alerts, providing functions like `ext… |
| `@reaatech/agent-runbook-analyzer` | published v0.1.0 | Scans a service repository to detect its language, framework, deployment platform, configuration files, entry points, API endpoints, external service connections, and package de… |
| `@reaatech/agent-runbook-cli` | published v0.1.0 | A CLI and programmatic entry point that generates operator runbooks from service repositories using AI analysis, providing five commands (`analyze`, `generate`, `validate`, `exp… |
| `@reaatech/agent-runbook-dashboards` | published v0.1.0 | Generates Grafana and CloudWatch dashboard configurations by scanning a codebase to identify relevant service metrics and producing complete dashboard panels with queries, thres… |
| `@reaatech/agent-runbook-failure-modes` | published v0.1.0 | Identifies potential failure points in a codebase by analyzing code patterns, categorizes them into 10 types (e.g., dependency, security, database) with severity scores, and gen… |
| `@reaatech/agent-runbook-health-checks` | published v0.1.0 | Identifies existing health check endpoints in a codebase and generates liveness, readiness, and startup probe definitions for Kubernetes and load balancers. Exports functions li… |
| `@reaatech/agent-runbook-incident` | published v0.1.0 | Generates SEV1–SEV4 incident response workflows, escalation policies, and communication templates for the Agent Runbook Generator. Exports functions like `generateIncidentWorkfl… |
| `@reaatech/agent-runbook-mcp` | published v0.1.0 | An MCP server that exposes 16 tools for analyzing repository structure, generating operator runbooks, and validating runbook completeness, consumable by AI coding agents via std… |
| `@reaatech/agent-runbook-observability` | published v0.1.0 | Provides structured logging via Pino, distributed tracing via OpenTelemetry, and Prometheus-compatible metrics for tracking runbook generation, agent costs, and quality. Exports… |
| `@reaatech/agent-runbook-rollback` | published v0.1.0 | Exports functions (`analyzeDeployment`, `generateRollbackProcedures`, `generateVerificationSteps`) that analyze a deployment configuration and produce structured rollback proced… |
| `@reaatech/agent-runbook-runbook` | published v0.1.0 | Assembles analysis results from the Agent Runbook Generator pipeline into a structured operator runbook with auto-generated table of contents, cross-references, and validation,… |
| `@reaatech/agent-runbook-service-map` | published v0.1.0 | Exports functions (`analyzeDependencies`, `generateServiceMap`, `exportGraph`) that analyze inter-service dependencies in a codebase, build directed dependency graphs with criti… |
| `@reaatech/agent-runbook-e2e` | pending npm |  |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-runbook-generator`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-runbook-generator
- Browse packages: https://reaatech.com/products/reliability-ops/agent-runbook-generator/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai-agents, alerts, automation, cli, code-analysis, devops, documentation, incident-response, observability, runbook-generators, sre, typescript
