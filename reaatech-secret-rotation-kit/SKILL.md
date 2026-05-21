---
name: reaatech-secret-rotation-kit
description: "These packages provide a framework for automating secret rotation across AWS Secrets Manager, GCP Secret Manager, and HashiCorp Vault. They solve the risk of service outages during credential updates by orchestrating overlapping key vali…"
license: MIT
---

# REAA secret-rotation-kit

These packages provide a framework for automating secret rotation across AWS Secrets Manager, GCP Secret Manager, and HashiCorp Vault. They solve the risk of service outages during credential updates by orchestrating overlapping key validity windows, propagation verification, and automatic rollbacks. The system uses a modular architecture where a core engine consumes provider-specific adapters and an optional HTTP sidecar to manage the full secret lifecycle.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Reliability & Ops** category. 7 packages live under `@reaatech/secret-rotation-core` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/secret-rotation-core` | pending npm | Orchestrates zero-downtime secret rotation lifecycles, including propagation verification, rollback logic, and state management. It provides a `RotationManager` class that requi… |
| `@reaatech/secret-rotation-observability` | pending npm | Provides structured JSON logging and a Prometheus-compatible metrics registry for the Secret Rotation Kit. It exports `LoggerService` and `MetricsService` classes that generate… |
| `@reaatech/secret-rotation-provider-aws` | pending npm | AWS Secrets Manager adapter for the Secret Rotation Kit, providing a `SecretProvider` implementation as a class (`AWSProvider`) that handles CRUD, version management (AWSCURRENT… |
| `@reaatech/secret-rotation-provider-gcp` | pending npm | A class (`GCPProvider`) that implements the `SecretProvider` interface from the Secret Rotation Kit, backed by the ` |
| `@reaatech/secret-rotation-provider-vault` | pending npm | Provides a `VaultProvider` class that implements the `SecretProvider` interface for HashiCorp Vault KV v2 engines. It requires the `node-vault` package at runtime to facilitate… |
| `@reaatech/secret-rotation-sidecar` | pending npm | Exposes a REST API and SSE stream for managing secret rotations, health checks, and Prometheus metrics. It provides a `SidecarServer` class that wraps a `RotationManager` instan… |
| `@reaatech/secret-rotation-types` | pending npm | Provides TypeScript interfaces, abstract base classes, and error definitions for building custom secret rotation providers and consumers. This package contains no runtime code a… |

## Quick start

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-secret-rotation-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/secret-rotation-kit
- Browse packages: https://reaatech.com/products/reliability-ops/secret-rotation-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: aws, developer-tools, devops, docker, security, typescript
