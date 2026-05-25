---
name: reaatech-secret-rotation-kit
description: "These packages give you a zero-downtime secret rotation engine that orchestrates the full lifecycle—generate, propagate, verify, activate, and revoke—across AWS Secrets Manager, GCP Secret Manager, HashiCorp Vault, and Vercel environment…"
license: MIT
---

# REAA secret-rotation-kit

These packages give you a zero-downtime secret rotation engine that orchestrates the full lifecycle—generate, propagate, verify, activate, and revoke—across AWS Secrets Manager, GCP Secret Manager, HashiCorp Vault, and Vercel environment variables. You'd adopt them to solve the operational problem of rotating secrets in production without causing outages when consumers haven't picked up the new key yet. The most distinctive thing is the overlapping key window design combined with dual verification strategies (provider-level polling and consumer-level active verification), all exposed through a pluggable provider interface and an optional HTTP sidecar that runs with zero code.

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
npm install @reaatech/secret-rotation-core @reaatech/secret-rotation-observability @reaatech/secret-rotation-provider-aws @reaatech/secret-rotation-provider-gcp @reaatech/secret-rotation-provider-vault @reaatech/secret-rotation-provider-vercel @reaatech/secret-rotation-sidecar @reaatech/secret-rotation-types
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/secret-rotation-core` | published v0.1.0 | A zero-downtime secret rotation engine that orchestrates the full lifecycle (generate → propagate → verify → activate → revoke) with overlapping key windows, dual verification s… |
| `@reaatech/secret-rotation-observability` | published v0.1.0 | A structured JSON logger and Prometheus-format metrics registry with zero runtime dependencies, providing `LoggerService` and `MetricsService` classes that implement the `Logger… |
| `@reaatech/secret-rotation-provider-aws` | published v0.1.0 | An AWS Secrets Manager adapter for the Secret Rotation Kit, implementing the `SecretProvider` interface with CRUD operations, version stage management (`AWSCURRENT`, `AWSPENDING… |
| `@reaatech/secret-rotation-provider-gcp` | published v0.1.0 | GCP Secret Manager adapter for the Secret Rotation Kit, implementing the `SecretProvider` interface with CRUD, versioning, rotation sessions, and health checks via the `@google-… |
| `@reaatech/secret-rotation-provider-vault` | published v0.1.0 | A HashiCorp Vault KV v2 adapter for the Secret Rotation Kit, implementing the `SecretProvider` interface with CRUD, versioning, rotation sessions, and health checks. It provides… |
| `@reaatech/secret-rotation-provider-vercel` | published v0.1.0 | A Vercel-specific `SecretProvider` implementation for the Secret Rotation Kit that manages environment variables via the Vercel REST API using only the global `fetch`. It provid… |
| `@reaatech/secret-rotation-sidecar` | published v0.1.0 | HTTP sidecar server that exposes secret rotation operations, health checks, Prometheus metrics, and SSE event streaming over a REST API, built on Node.js's built-in `http` modul… |
| `@reaatech/secret-rotation-types` | published v0.1.0 | Type definitions, abstract interfaces, and error classes for the Secret Rotation Kit ecosystem, providing shared types like `SecretKey`, `RotationState`, `SecretProvider`, and `… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-secret-rotation-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/secret-rotation-kit
- Browse packages: https://reaatech.com/products/reliability-ops/secret-rotation-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: aws, developer-tools, devops, docker, security, typescript
