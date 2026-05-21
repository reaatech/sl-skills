---
name: reaatech-voice-agent-kit
description: "These packages provide a transport layer for real-time voice AI, orchestrating the pipeline between telephony streams, speech-to-text, text-to-speech, and MCP-based agent logic. They solve the challenge of maintaining sub-second latency…"
license: MIT
---

# REAA voice-agent-kit

These packages provide a transport layer for real-time voice AI, orchestrating the pipeline between telephony streams, speech-to-text, text-to-speech, and MCP-based agent logic. They solve the challenge of maintaining sub-second latency in conversational systems by enforcing strict timing budgets and providing provider-agnostic adapters for services like Deepgram, AWS, and Google Cloud. The architecture centers on an event-driven pipeline that decouples telephony handling from agent decision-making, allowing you to swap providers or integrate custom MCP servers without modifying core orchestration logic.

## When to use this

> _Editorial copy pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._
>
> Reach for this family when working in the **Domain Pipelines** category. 5 packages live under `@reaatech/voice-agent-core` and siblings.

## Packages

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/voice-agent-core` | pending npm | Orchestrates an end-to-end STT→MCP→TTS voice agent pipeline with session management, per-stage latency enforcement, and OpenTelemetry observability, exposing an event-emitting `… |
| `@reaatech/voice-agent-mcp-client` | pending npm | Connects to Model Context Protocol (MCP) servers via JSON-RPC 2.0 to manage tool discovery, conversation history, and request retries. It provides an `MCPClient` class that retu… |
| `@reaatech/voice-agent-stt` | pending npm | Provides a unified interface for streaming audio to Deepgram, AWS Transcribe, or Google Cloud Speech-to-Text via provider-specific classes. It includes built-in utilities for au… |
| `@reaatech/voice-agent-telephony` | pending npm | Provides a `TwilioMediaStreamHandler` class that manages the bidirectional Twilio Media Streams WebSocket protocol, including audio buffering, barge-in detection, and event life… |
| `@reaatech/voice-agent-tts` | pending npm | Provider-agnostic text-to-speech interface using `AsyncIterable<AudioChunk>` for streaming audio, with built-in adapters for Deepgram Aura, AWS Polly, and Google Cloud TTS, plus… |

## Quick start

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

> _Code example pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Don't reach for this when

> _Pending — see [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the hybrid AI-draft + admin review flow._

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-voice-agent-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/voice-agent-kit
- Browse packages: https://reaatech.com/products/domain-pipelines/voice-agent-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, calcom, conversational-ai, latency-optimization, rag, real-time, speech-to-text, stt, telephony, test-to-speech, tts, twillio, typescript, vector-search, voice-agent, voice-ai
