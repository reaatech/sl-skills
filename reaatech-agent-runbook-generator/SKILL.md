---
name: reaatech-agent-runbook-generator
description: "These packages automate the creation of operator runbooks by scanning service repositories to generate alerts, dashboards, failure modes, and incident response workflows. They solve the problem of maintaining outdated or missing document…"
license: MIT
---

# REAA agent-runbook-generator

These packages automate the creation of operator runbooks by scanning service repositories to generate alerts, dashboards, failure modes, and incident response workflows. They solve the problem of maintaining outdated or missing documentation by programmatically deriving operational context directly from your codebase and infrastructure configurations. The collection functions as a modular pipeline, using a shared set of Zod schemas and an MCP server to integrate these generation tasks into AI-assisted development environments like Cursor or Claude Code.

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
| `@reaatech/agent-runbook` | published v0.1.0 | Provides shared domain types, Zod schemas, utility functions, and error classes for the Agent Runbook Generator ecosystem—exported as a collection of TypeScript interfaces, enum… |
| `@reaatech/agent-runbook-agent` | published v0.1.0 | Provides a unified interface for interacting with Anthropic, OpenAI, and Google Gemini models to perform automated repository analysis and runbook generation. It exposes an `Ana… |
| `@reaatech/agent-runbook-alerts` | published v0.1.0 | Extracts existing monitoring configurations and generates new SLO-based, resource, and application alert definitions for Prometheus, Datadog, and CloudWatch. It provides a colle… |
| `@reaatech/agent-runbook-analyzer` | published v0.1.0 | Scans a service repository to detect its language, framework, deployment platform, configuration, API endpoints, and dependencies, returning analysis objects from functions like… |
| `@reaatech/agent-runbook-cli` | published v0.1.0 | Provides a CLI and programmatic interface to analyze service repositories and generate operator runbooks using LLM-based orchestration. It also functions as a central barrel pac… |
| `@reaatech/agent-runbook-dashboards` | published v0.1.0 | Identifies relevant service metrics from code and generates dashboard configurations for Grafana or CloudWatch, exposing functions like `identifyMetrics` and `generateDashboard`… |
| `@reaatech/agent-runbook-failure-modes` | published v0.1.0 | Analyzes local codebases to identify failure points and generates corresponding mitigation strategies like retry policies and circuit breaker configurations. It provides a set o… |
| `@reaatech/agent-runbook-health-checks` | published v0.1.0 | Analyzes codebases to identify existing health check patterns and generates Kubernetes probe YAML, load balancer configurations, and endpoint implementations. It provides a set… |
| `@reaatech/agent-runbook-incident` | published v0.1.0 | Generates severity-based incident response workflows, escalation policies, and communication templates for automated runbooks. It provides a set of utility functions for configu… |
| `@reaatech/agent-runbook-mcp` | published v0.1.0 | Exposes runbook analysis, generation, and validation capabilities as an MCP server that provides 16 tools for AI agents. It offers a `RunbookMCPServer` class and a factory funct… |
| `@reaatech/agent-runbook-observability` | published v0.1.0 | Provides a unified observability layer for agent-based runbook generation, offering a set of initialization functions and helpers for Pino logging, OpenTelemetry tracing, and Pr… |
| `@reaatech/agent-runbook-rollback` | published v0.1.0 | Generates platform-specific rollback procedures, pre-checks, and verification plans by analyzing deployment configurations. It provides a set of utility functions to output auto… |
| `@reaatech/agent-runbook-runbook` | published v0.1.0 | Assembles and formats infrastructure runbooks from analysis data using a collection of pipeline functions, template utilities, and multi-format exporters. It provides tools to v… |
| `@reaatech/agent-runbook-service-map` | published v0.1.0 | Analyzes inter-service dependencies from a repository path to generate directed graphs and perform critical path analysis. It provides a set of utility functions to export these… |
| `@reaatech/agent-runbook-e2e` | pending npm |  |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-agent-runbook-generator`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/agent-runbook-generator
- Browse packages: https://reaatech.com/products/reliability-ops/agent-runbook-generator/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai-agents, alerts, automation, cli, code-analysis, devops, documentation, incident-response, observability, runbook-generators, sre, typescript
