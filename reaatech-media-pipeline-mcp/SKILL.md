---
name: reaatech-media-pipeline-mcp
description: "These packages provide a framework for building multi-step media processing workflows, such as image generation, audio transcription, and document extraction, as chainable tools within an MCP server. You would adopt them to automate comp…"
license: MIT
---

# REAA media-pipeline-mcp

These packages provide a framework for building multi-step media processing workflows, such as image generation, audio transcription, and document extraction, as chainable tools within an MCP server. You would adopt them to automate complex media pipelines that require artifact passing, quality validation, and multi-provider orchestration. The system is designed around a unified pipeline engine that uses variable interpolation to pass outputs between steps while enforcing quality gates and resilience patterns like circuit breakers.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Domain Pipelines** category. 20 packages live under `@reaatech/media-pipeline-mcp-anthropic` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/media-pipeline-mcp-anthropic` | published v0.3.0 | Provides an Anthropic-backed implementation of the media pipeline framework for image analysis, OCR, and structured data extraction. It exposes an `AnthropicProvider` class that… |
| `@reaatech/media-pipeline-mcp-audio-gen` | published v0.3.0 | Provides a set of operations for audio generation (text-to-speech, speech-to-text, diarization, source separation, music and sound effect generation) as a `create |
| `@reaatech/media-pipeline-mcp-deepgram` | published v0.3.0 | A Deepgram provider for the media pipeline framework that implements `audio.stt` and `audio.diarize` operations using the Nova-2 model, exposing a `DeepgramProvider` class usabl… |
| `@reaatech/media-pipeline-mcp-doc-extraction` | published v0.3.0 | Provides an interface for performing OCR, table extraction, structured field parsing, and document summarization by delegating tasks to vision-capable LLMs. It exposes these cap… |
| `@reaatech/media-pipeline-mcp-elevenlabs` | published v0.3.0 | An ElevenLabs provider for the media-pipeline MCP framework, exporting the `ElevenLabsProvider` class that executes `audio.tts` operations with voice selection, speed, and fine-… |
| `@reaatech/media-pipeline-mcp-fal` | published v0.3.0 | A FalProvider class that implements the media pipeline provider interface, enabling image generation (Fast Flux Pro), upscaling, background removal, and video generation via the… |
| `@reaatech/media-pipeline-mcp-google` | published v0.3.0 | A Google Cloud provider class that uses Document AI for OCR, table extraction, and field extraction, and Vertex AI Gemini |
| `@reaatech/media-pipeline-mcp-image-edit` | published v0.3.0 | Provides local image editing (resize, crop, composite) via Sharp and delegates |
| `@reaatech/media-pipeline-mcp-observability` | published v0.3.0 | A service facade (`createObservabilityService`) that bundles OpenTelemetry tracing, Prometheus metrics, structured JSON logging, |
| `@reaatech/media-pipeline-mcp-openai` | published v0.3.0 | An OpenAI provider class (`OpenAIProvider`) that integrates image generation (DALL·E 3), image description (GPT‑4o Vision), text‑to‑speech (TTS‑1), and speech‑to‑text (Whisper‑1… |
| `@reaatech/media-pipeline-mcp-pipeline` | published v0.3.0 | Orchestrates multi-step media workflows through template management, variable interpolation, and step validation. It provides a `PipelineOperations` class that handles the insta… |
| `@reaatech/media-pipeline-mcp-provider-core` | published v0.3.0 | Provides an abstract `MediaProvider` base class and shared TypeScript interfaces to standardize the implementation of media generation backends. It includes built-in utilities f… |
| `@reaatech/media-pipeline-mcp-replicate` | published v0.3.0 | Provides a Replicate-backed provider class for the media-pipeline framework, enabling image, audio, and video processing tasks like upscaling, background removal, and generation… |
| `@reaatech/media-pipeline-mcp-resilience` | published v0.3.0 | Implements circuit breaker and exponential backoff retry patterns to protect downstream services from cascading failures. It provides factory functions for creating configurable… |
| `@reaatech/media-pipeline-mcp-security` | published v0.3.0 | Provides authentication, role-based access control, token-bucket rate limiting, and audit logging for media pipeline services. It exports factory functions and classes to integr… |
| `@reaatech/media-pipeline-mcp-server` | published v0.3.0 | An MCP server that exposes 30+ media operations (image generation/editing, audio TTS/STT, video, document extraction) as JSON-RPC 2.0 tools over StreamableHTTP transport. It pro… |
| `@reaatech/media-pipeline-mcp-stability` | published v0.3.0 | A Stability AI provider that exposes a ` |
| `@reaatech/media-pipeline-mcp-storage` | published v0.3.0 | Provides a unified `ArtifactStore` interface for persisting and retrieving |
| `@reaatech/media-pipeline-mcp-video-gen` | published v0.3.0 | Provides a `createVideoGenOperations` function that returns a class with methods for text-to-video and image-to-video generation via provider delegation (e.g., Kling), plus loca… |
| `@reaatech/media-pipeline-mcp` | pending npm | Orchestrates media processing workflows through a `PipelineExecutor` class that handles sequential step execution, artifact tracking, and quality gate validation. It provides a… |

## Quick start

```bash
npm install @reaatech/media-pipeline-mcp-anthropic @reaatech/media-pipeline-mcp-audio-gen @reaatech/media-pipeline-mcp-deepgram @reaatech/media-pipeline-mcp-doc-extraction @reaatech/media-pipeline-mcp-elevenlabs @reaatech/media-pipeline-mcp-fal @reaatech/media-pipeline-mcp-google @reaatech/media-pipeline-mcp-image-edit @reaatech/media-pipeline-mcp-observability @reaatech/media-pipeline-mcp-openai @reaatech/media-pipeline-mcp-pipeline @reaatech/media-pipeline-mcp-provider-core @reaatech/media-pipeline-mcp-replicate @reaatech/media-pipeline-mcp-resilience @reaatech/media-pipeline-mcp-security @reaatech/media-pipeline-mcp-server @reaatech/media-pipeline-mcp-stability @reaatech/media-pipeline-mcp-storage @reaatech/media-pipeline-mcp-video-gen
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-media-pipeline-mcp`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/media-pipeline-mcp
- Browse packages: https://reaatech.com/products/domain-pipelines/media-pipeline-mcp/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, ai-tools, audio-generation, content-generation, image-generation, mcp, media-pipeline, multimodal, ocr, quality-gates, stt, tts, typescript, video-generation, workflow-automation
