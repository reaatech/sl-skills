---
name: reaatech-media-pipeline-mcp
description: "These packages give you a set of MCP tools for generating and processing images, audio, video, documents, and 3D models, then chaining those operations into pipelines with artifact passing between steps. You would adopt them to avoid wir…"
license: MIT
---

# REAA media-pipeline-mcp

These packages give you a set of MCP tools for generating and processing images, audio, video, documents, and 3D models, then chaining those operations into pipelines with artifact passing between steps. You would adopt them to avoid wiring together multiple provider SDKs, retry logic, cost tracking, and caching yourself when building AI agents that produce media as output. The most distinctive thing is that every operation is a composable pipeline step with built-in quality gates, budget enforcement, content-addressed caching, and multi-provider routing, so you can define a multi-step workflow once and have it handle provider fallback, cost caps, and resumable execution automatically.

## When to use this

Reach for the media-pipeline-mcp family whenever a user request involves chaining multiple media operations—for example, generating an image then describing it, transcribing audio then extracting tables from a document, or applying quality gates after video generation. The core value is orchestrating provider-agnostic steps with artifact passing, variable interpolation, and built-in resilience (circuit breakers, retry, cost tracking). You should use it when the prompt contains phrases like "generate and then edit an image", "transcribe audio and extract tables from a document", "run a multi-step media pipeline with quality checks", or "create a video from a script and add subtitles". This family solves the problem of composing heterogeneous media operations into a single, traceable, cost-accounted pipeline that can survive provider failures and enforce content quality before proceeding.

It is also the right choice when you need to expose media workflows as MCP tools for AI agents. The @reaatech/media-pipeline-mcp-server package provides a ready-made MCP server with 35+ operations, provider routing, webhooks, and multi-tenant key management—deploy it as a single binary and agents can call pipelines via standard JSON-RPC 2.0 tools.

## Quick start example

The following example creates a pipeline with two steps: generate an image from a prompt, then describe the generated image using a vision provider. It uses the core pipeline engine from `@reaatech/media-pipeline-mcp-core` and the pipeline definition from `@reaatech/media-pipeline-mcp-pipeline`.

```typescript
import { PipelineEngine } from '@reaatech/media-pipeline-mcp-core';
import { createPipeline } from '@reaatech/media-pipeline-mcp-pipeline';
import { OpenAIProvider } from '@reaatech/media-pipeline-mcp-openai';

const openai = new OpenAIProvider({ apiKey: process.env.OPENAI_API_KEY });

const pipeline = createPipeline({
  steps: [
    { operation: 'image.gen', provider: openai, params: { prompt: 'A cat on a mat', model: 'dall-e-3' }, outputs: { imageRef: 'result.url' } },
    { operation: 'image.describe', provider: openai, params: { imageUrl: '{{steps[0].imageRef}}' } }
  ],
  qualityGates: [],
});

const engine = new PipelineEngine({ artifactStore: /* ... */ });
const result = await engine.execute({ pipeline, tenantId: 'user-1' });
console.log(result.steps[1].output);
```

The pipeline engine injects the generated image URL from step 0 into step 1 via `{{steps[0].imageRef}}`. Each step is executed sequentially, and artifacts are stored in the configured store. For full MCP server integration, use `@reaatech/media-pipeline-mcp-server` instead of building your own engine.

## Don't reach for this when

- **Single-step media operation** – If the user only needs one image generation or one transcription, use the individual provider package directly (e.g., `@reaatech/media-pipeline-mcp-openai` for DALL·E) rather than the full pipeline framework. No pipeline orchestration overhead needed.
- **Non-media workflows** – This family is designed for media artifacts (images, audio, video, documents). For general-purpose task chains (e.g., code generation, data transformation, API orchestration), use a generic MCP framework like `@modelcontextprotocol/sdk` directly, or a workflow system like `@reaatech/agent-workflow` if available.
- **Low-latency, real-time processing** – The pipeline engine adds variable interpolation and artifact registry overhead. For sub-second latency requirements (e.g., live audio streaming), use provider-specific SDKs with WebSocket support and skip the pipeline abstraction.
- **Stateful long-running workflows with human-in-the-loop** – The pipeline is linear, sequential, and auto-resumable via persistence, but it does not support branching, waiting for external input, or manual approval steps. For such flows, pair with a dedicated workflow engine like Temporal or use `@reaatech/human-in-the-loop-mcp` if available.
- **Simple MCP tool serving** – If you only need to expose a few unrelated media operations as MCP tools without pipeline chaining

## Packages

```bash
npm install @reaatech/media-pipeline-mcp-anthropic @reaatech/media-pipeline-mcp-audio-gen @reaatech/media-pipeline-mcp-comfyui @reaatech/media-pipeline-mcp-core @reaatech/media-pipeline-mcp-cost @reaatech/media-pipeline-mcp-deepgram @reaatech/media-pipeline-mcp-doc-extraction @reaatech/media-pipeline-mcp-elevenlabs @reaatech/media-pipeline-mcp-fal @reaatech/media-pipeline-mcp-google @reaatech/media-pipeline-mcp-image-edit @reaatech/media-pipeline-mcp-keyvault @reaatech/media-pipeline-mcp-luma @reaatech/media-pipeline-mcp-meshy @reaatech/media-pipeline-mcp-observability @reaatech/media-pipeline-mcp-ollama @reaatech/media-pipeline-mcp-openai @reaatech/media-pipeline-mcp-persistence @reaatech/media-pipeline-mcp-pipeline @reaatech/media-pipeline-mcp-provenance @reaatech/media-pipeline-mcp-provider-core @reaatech/media-pipeline-mcp-replicate @reaatech/media-pipeline-mcp-resilience @reaatech/media-pipeline-mcp-security @reaatech/media-pipeline-mcp-server @reaatech/media-pipeline-mcp-stability @reaatech/media-pipeline-mcp-storage @reaatech/media-pipeline-mcp-video-gen
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/media-pipeline-mcp-anthropic` | published v0.3.0 | An Anthropic provider for the media pipeline framework that wraps Claude Sonnet's vision models to perform image description, OCR, table extraction, structured field extraction,… |
| `@reaatech/media-pipeline-mcp-audio-gen` | published v0.3.0 | A factory function that creates an `AudioGenOperations` instance providing text-to-speech, speech-to-text, speaker diarization, source separation, music generation, and sound ef… |
| `@reaatech/media-pipeline-mcp-comfyui` | published v0.3.0 | A ComfyUI provider for the media pipeline framework that runs image generation, image editing, and video generation on your own GPU via local ComfyUI workflows, with zero API co… |
| `@reaatech/media-pipeline-mcp-core` | published v0.3.0 | Core framework for media pipeline orchestration, providing a Zod-validated type system, pipeline execution engine with variable interpolation, quality gate evaluation, artifact… |
| `@reaatech/media-pipeline-mcp-cost` | published v0.3.0 | A typed cost ledger for tracking per-operation expenses in a media pipeline, providing an `InMemoryCostLedger` class with `charge()`, `preflight()`, `totalForRun()`, and `totalF… |
| `@reaatech/media-pipeline-mcp-deepgram` | published v0.3.0 | A Deepgram provider for the media-pipeline framework that exposes `audio.stt` and `audio.diarize` operations via a `DeepgramProvider` class, using Nova-2 for speech-to-text tran… |
| `@reaatech/media-pipeline-mcp-doc-extraction` | published v0.3.0 | A factory function (`createDocumentExtractionOperations`) that returns a `DocumentExtractionOperations` instance providing OCR, table extraction, schema-driven field extraction,… |
| `@reaatech/media-pipeline-mcp-elevenlabs` | published v0.3.0 | An ElevenLabs provider for the media pipeline framework that exposes a `MediaProvider` class (`ElevenLabsProvider`) with `execute`, `healthCheck`, and `estimateCost` methods for… |
| `@reaatech/media-pipeline-mcp-fal` | published v0.3.0 | A Fal.ai provider for the media pipeline framework that exposes a `FalProvider` class supporting image generation, upscaling, background removal, text-to-video, and image-to-vid… |
| `@reaatech/media-pipeline-mcp-google` | published v0.3.0 | A Google Cloud provider for the media-pipeline framework that exposes Document AI (OCR, table extraction, field extraction) and Vertex AI Gemini (image description) as a unified… |
| `@reaatech/media-pipeline-mcp-image-edit` | published v0.3.0 | An image editing operations factory that provides Sharp-based local processing (resize, crop, composite) and provider-delegated operations (upscale, background removal, inpainti… |
| `@reaatech/media-pipeline-mcp-keyvault` | published v0.3.0 | Multi-tenant API key vault that resolves tenant-scoped provider credentials from AWS Secrets Manager, GCP Secret Manager, environment variables, or in-memory storage, exposing `… |
| `@reaatech/media-pipeline-mcp-luma` | published v0.3.0 | A Luma AI provider for the media pipeline framework that generates 3D meshes from text descriptions via the Dream Machine API, exposing a `LumaProvider` class with `execute()`,… |
| `@reaatech/media-pipeline-mcp-meshy` | published v0.3.0 | A Meshy-4 provider for the media-pipeline framework that generates 3D meshes from text prompts or reference images, supporting PBR textures and GLB/FBX/OBJ/USDZ output formats.… |
| `@reaatech/media-pipeline-mcp-observability` | published v0.3.0 | Provides a single `ObservabilityService` facade that bundles OpenTelemetry tracing with auto-instrumentation, Prometheus-compatible metrics, structured Pino-compatible logging,… |
| `@reaatech/media-pipeline-mcp-ollama` | published v0.3.0 | A local Ollama provider for the media-pipeline framework that exposes text completion, embedding generation, and image description via any Ollama model. It provides an `OllamaPr… |
| `@reaatech/media-pipeline-mcp-openai` | published v0.3.0 | An OpenAI provider for the media-pipeline framework that exposes a class (`OpenAIProvider`) supporting DALL-E 3 image generation, GPT-4o vision-based image description, TTS-1 te… |
| `@reaatech/media-pipeline-mcp-persistence` | published v0.3.0 | Provides in-memory and Redis-backed pipeline state store abstractions with optimistic locking, run lifecycle management, event log persistence, and tenant-scoped queries for mul… |
| `@reaatech/media-pipeline-mcp-pipeline` | published v0.3.0 | A set of functions for creating, validating, and executing media processing pipelines with pre-built templates, variable interpolation, batch processing, aspect-ratio fan-out, a… |
| `@reaatech/media-pipeline-mcp-provenance` | published v0.3.0 | A C2PA content provenance signing library for AI-generated media that embeds tamper-evident manifests naming the model, pipeline DAG, and operator. Exports a `ProvenanceSigner`… |
| `@reaatech/media-pipeline-mcp-provider-core` | published v0.3.0 | Abstract base class and shared interfaces for media provider implementations, providing a `MediaProvider` abstract class with deterministic caching, retry with exponential backo… |
| `@reaatech/media-pipeline-mcp-replicate` | published v0.3.0 | A Replicate provider for the media-pipeline framework that exposes image upscaling, background removal, inpainting, audio source separation, text-to-video, and image-to-video op… |
| `@reaatech/media-pipeline-mcp-resilience` | published v0.3.0 | A three-state circuit breaker and retry policy with exponential backoff and jitter for protecting downstream media pipeline providers from cascading failures. Provides `createCi… |
| `@reaatech/media-pipeline-mcp-security` | published v0.3.0 | Provides authentication (API keys with constant-time comparison, JWT/OAuth2 with HS256), role-based access control (admin/operator/viewer with 10 permissions), token bucket rate… |
| `@reaatech/media-pipeline-mcp-server` | published v0.3.0 | An MCP server that exposes 35+ media operations (image generation/editing, audio TTS/STT, video generation, document extraction, 3D mesh generation) as JSON-RPC 2.0 tools over S… |
| `@reaatech/media-pipeline-mcp-stability` | published v0.3.0 | A Stability AI provider for the media pipeline framework that generates images via the Stable Image v2beta REST API using SD3, SDXL, and SD1.5 models, exposing a `StabilityProvi… |
| `@reaatech/media-pipeline-mcp-storage` | published v0.3.0 | A unified `ArtifactStore` interface (`put`, `get`, `getSignedUrl`, `delete`, `list`, `healthCheck`) backed by local filesystem, AWS S3, or Google Cloud Storage, selected via a t… |
| `@reaatech/media-pipeline-mcp-video-gen` | published v0.3.0 | A factory function (`createVideoGenOperations`) that returns a `VideoGenOperations` instance for text-to-video and image-to-video generation via pluggable providers (e.g., Repli… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-media-pipeline-mcp`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/media-pipeline-mcp
- Browse packages: https://reaatech.com/products/domain-pipelines/media-pipeline-mcp/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai-tools, audio-generation, content-generation, image-generation, mcp, media-pipeline, multimodal, ocr, quality-gates, stt, tts, typescript, video-generation, workflow-automation
