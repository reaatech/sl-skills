---
name: reaatech-secret-rotation-kit
description: "These packages provide a framework for automating secret rotation across AWS Secrets Manager, GCP Secret Manager, and HashiCorp Vault. They solve the risk of service outages during credential updates by orchestrating overlapping key vali…"
license: MIT
---

# REAA secret-rotation-kit

These packages provide a framework for automating secret rotation across AWS Secrets Manager, GCP Secret Manager, and HashiCorp Vault. They solve the risk of service outages during credential updates by orchestrating overlapping key validity windows, propagation verification, and automatic rollbacks. The system uses a modular architecture where a core engine consumes provider-specific adapters and an optional HTTP sidecar to manage the full secret lifecycle.

## When to use this

Reach for the **secret-rotation-kit** family when the task calls for automated, zero-downtime rotation of secrets stored in AWS Secrets Manager, GCP Secret Manager, or HashiCorp Vault. This kit solves the specific problem of updating credentials without dropping service connections: it manages overlapping validity windows (e.g., pending → current → previous labels), waits for propagation, and automatically rolls back on failure.

Trigger phrases that map to this family:
- "rotate database password in AWS Secrets Manager without downtime"
- "set up automatic secret rotation for Vault KV v2 engine"
- "need overlapping credential windows during secret updates"
- "orchestrate secret lifecycle with rollback on propagation failure"

The core engine (`@reaatech/secret-rotation-core`) is provider-agnostic; you plug in a specific adapter (AWS, GCP, or Vault) to handle the CRUD and version management. An optional HTTP sidecar (`@reaatech/secret-rotation-sidecar`) exposes REST endpoints and SSE streams for external coordination. Use this family when your architecture requires scriptable, modular rotation logic that integrates with observability (Prometheus metrics, structured JSON logs).

## Quick start example

The `RotationManager` from `@reaatech/secret-rotation-core` combines a provider adapter and an observability service to rotate a secret through three overlapping versions.

```typescript
import { RotationManager } from '@reaatech/secret-rotation-core';
import { AWSProvider } from '@reaatech/secret-rotation-provider-aws';
import { LoggerService, MetricsService } from '@reaatech/secret-rotation-observability';

const provider = new AWSProvider({ region: 'us-east-1' });
const logger = new LoggerService({ service: 'secret-rotator' });
const metrics = new MetricsService({ prefix: 'rotation_' });

const manager = new RotationManager({
  provider,
  logger,
  metrics,
  secretId: 'my-db-password',
  rotationWindowMs: 30_000, // 30-second overlap for old and new
  propagationCheckIntervalMs: 5_000,
  maxPropagationRetries: 3,
});

await manager.rotate({ rollbackOnFailure: true });
```

The `rotate()` method creates a new version (AWSPENDING), waits for the deployment to propagate, then promotes it to AWSCURRENT and demotes the old version to AWSPREVIOUS. If the propagation check fails, it automatically rolls back by restoring the previous current version.

## Don't reach for this when

- **You only need to read or store a secret once.** The kit is overkill for simple fetch/store operations. Use the native AWS SDK (`@aws-sdk/client-secrets-manager`) or `node-vault` directly.
- **Your secrets are managed by a non‑supported store (e.g., Azure Key Vault, Kubernetes Secrets).** The kit only supports AWS, GCP, and Vault adapters. For other stores, write a custom provider implementing the `SecretProvider` interface from `@reaatech/secret-rotation-types`.
- **You want a one‑click managed rotation service with no code.** The kit is a programmatic framework; it requires you to write rotation logic (or use the sidecar). Consider AWS Secrets Manager’s built‑in rotation or Vault’s built‑in rotation for fully managed experiences.
- **You need real‑time secret injection into running containers (e.g., Kubernetes secrets).** The kit rotates the stored secret but does not automatically push it to consuming pods or services. Use a sidecar injection tool like `external-secrets` or `secrets-store-csi-driver` for that.
- **Your rotation window is sub‑second or requires immediate propagation.** The kit relies on polling and retries; it is designed for loads where a 5‑30 second window is acceptable. For sub‑second rollover, consider database‑native credential rotation mechanisms.

## Packages

```bash
npm install @reaatech/secret-rotation-core @reaatech/secret-rotation-observability @reaatech/secret-rotation-provider-aws @reaatech/secret-rotation-provider-gcp @reaatech/secret-rotation-provider-vault @reaatech/secret-rotation-sidecar @reaatech/secret-rotation-types
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/secret-rotation-core` | published v0.1.0 | Orchestrates zero-downtime secret rotation lifecycles, including propagation verification, rollback logic, and state management. It provides a `RotationManager` class that requi… |
| `@reaatech/secret-rotation-observability` | published v0.1.0 | Provides structured JSON logging and a Prometheus-compatible metrics registry for the Secret Rotation Kit. It exports `LoggerService` and `MetricsService` classes that generate… |
| `@reaatech/secret-rotation-provider-aws` | published v0.1.0 | AWS Secrets Manager adapter for the Secret Rotation Kit, providing a `SecretProvider` implementation as a class (`AWSProvider`) that handles CRUD, version management (AWSCURRENT… |
| `@reaatech/secret-rotation-provider-gcp` | published v0.1.0 | A class (`GCPProvider`) that implements the `SecretProvider` interface from the Secret Rotation Kit, backed by the ` |
| `@reaatech/secret-rotation-provider-vault` | published v0.1.0 | Provides a `VaultProvider` class that implements the `SecretProvider` interface for HashiCorp Vault KV v2 engines. It requires the `node-vault` package at runtime to facilitate… |
| `@reaatech/secret-rotation-sidecar` | published v0.1.0 | Exposes a REST API and SSE stream for managing secret rotations, health checks, and Prometheus metrics. It provides a `SidecarServer` class that wraps a `RotationManager` instan… |
| `@reaatech/secret-rotation-types` | published v0.1.0 | Provides TypeScript interfaces, abstract base classes, and error definitions for building custom secret rotation providers and consumers. This package contains no runtime code a… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-secret-rotation-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/secret-rotation-kit
- Browse packages: https://reaatech.com/products/reliability-ops/secret-rotation-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: aws, developer-tools, devops, docker, security, typescript
