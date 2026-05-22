---
name: reaatech-voice-agent-kit
description: "These packages provide a transport layer for real-time voice AI, orchestrating the pipeline between telephony streams, speech-to-text, text-to-speech, and MCP-based agent logic. They solve the challenge of maintaining sub-second latency…"
license: MIT
---

# REAA voice-agent-kit

These packages provide a transport layer for real-time voice AI, orchestrating the pipeline between telephony streams, speech-to-text, text-to-speech, and MCP-based agent logic. They solve the challenge of maintaining sub-second latency in conversational systems by enforcing strict timing budgets and providing provider-agnostic adapters for services like Deepgram, AWS, and Google Cloud. The architecture centers on an event-driven pipeline that decouples telephony handling from agent decision-making, allowing you to swap providers or integrate custom MCP servers without modifying core orchestration logic.

## When to use this

Reach for this family when a user request involves building a real-time voice AI agent that must listen, think, and speak within conversational latency (sub-second round trips). Typical trigger phrases are "voice agent that can take or make phone calls", "STT → MCP → TTS pipeline with telephony", "real-time voice assistant using Twilio", or "low-latency speech-to-text and text-to-speech for an AI agent". The core problem is orchestrating a continuous audio stream from a telephony source (e.g., Twilio Media Stream) through speech recognition, agent reasoning via MCP tool calls, and back to speech synthesis, all while maintaining strict per-stage time budgets.

This family is particularly valuable when you need to swap cloud providers (Deepgram, AWS, Google) for STT or TTS without rewriting orchestration logic, or when you want to add custom MCP servers to control tool discovery and agent behaviour independently of the voice pipeline. The event-driven pipeline exposed by `@reaatech/voice-agent-core` decouples telephony handling from the agent decision loop, so you can reuse the same pipeline logic across different telephony providers or test with mock services.

## Quick start example

```typescript
import { Pipeline, Session } from '@reaatech/voice-agent-core';
import { TwilioMediaStreamHandler } from '@reaatech/voice-agent-telephony';
import { MCPClient } from '@reaatech/voice-agent-mcp-client';

const mcpClient = new MCPClient({ serverUrl: 'http://localhost:3001/mcp' });
const pipeline = new Pipeline({
  sttProvider: 'deepgram',
  ttsProvider: 'google',
  mcpClient,
  latencyBudgetMs: { stt: 400, mcp: 800, tts: 400 },
});

const handler = new TwilioMediaStreamHandler();
handler.onAudioStream(async (audioChunk) => {
  await pipeline.processAudio(audioChunk);
});
pipeline.start({ inboundStream: handler });
```

The example sets up a voice pipeline that accepts a Twilio Media Stream, routes audio through Deepgram for speech-to-text, passes transcripts to an MCP-based agent for decision-making, and synthesises replies via Google Cloud TTS. Latency budgets enforce that each stage completes within the allocated time.

## Don't reach for this when

- **You only need a text‑based chatbot without voice.** This family adds significant overhead for audio I/O. Use a simpler chat library like `@reaatech/agent-core` or a plain HTTP MCP client.
- **Your task processes pre‑recorded audio files (batch mode).** The entire design assumes streaming, real‑time audio. For batch speech‑to‑text or text‑to‑speech, use the underlying cloud SDKs directly (e.g., `@google-cloud/speech` or `aws-sdk`'s `Transcribe`).
- **You are building a full contact centre with routing, queuing, and analytics.** This family only handles the voice agent pipeline inside one call. For contact centre orchestration, consider a dedicated platform like Twilio Flex or Amazon Connect.
- **The user request is about building a voice interface for a web app (

## Packages

```bash
# (no packages published to npm yet — install from source or wait for publish)
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/voice-agent-core` | pending npm | Orchestrates an end-to-end STT→MCP→TTS voice agent pipeline with session management, per-stage latency enforcement, and OpenTelemetry observability, exposing an event-emitting `… |
| `@reaatech/voice-agent-mcp-client` | pending npm | Connects to Model Context Protocol (MCP) servers via JSON-RPC 2.0 to manage tool discovery, conversation history, and request retries. It provides an `MCPClient` class that retu… |
| `@reaatech/voice-agent-stt` | pending npm | Provides a unified interface for streaming audio to Deepgram, AWS Transcribe, or Google Cloud Speech-to-Text via provider-specific classes. It includes built-in utilities for au… |
| `@reaatech/voice-agent-telephony` | pending npm | Provides a `TwilioMediaStreamHandler` class that manages the bidirectional Twilio Media Streams WebSocket protocol, including audio buffering, barge-in detection, and event life… |
| `@reaatech/voice-agent-tts` | pending npm | Provider-agnostic text-to-speech interface using `AsyncIterable<AudioChunk>` for streaming audio, with built-in adapters for Deepgram Aura, AWS Polly, and Google Cloud TTS, plus… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-voice-agent-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/voice-agent-kit
- Browse packages: https://reaatech.com/products/domain-pipelines/voice-agent-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, calcom, conversational-ai, latency-optimization, rag, real-time, speech-to-text, stt, telephony, test-to-speech, tts, twillio, typescript, vector-search, voice-agent, voice-ai
