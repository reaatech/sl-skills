---
name: reaatech-voice-agent-kit
description: "These packages give you the full pipeline to turn microphone or phone audio into an AI agent response and back — speech-to-text, MCP tool calls, text-to-speech, and telephony or WebRTC transport, all orchestrated with per-stage latency b…"
license: MIT
---

# REAA voice-agent-kit

These packages give you the full pipeline to turn microphone or phone audio into an AI agent response and back — speech-to-text, MCP tool calls, text-to-speech, and telephony or WebRTC transport, all orchestrated with per-stage latency budgets. You'd adopt them to build a production voice agent without writing the audio plumbing, provider switching, or session management yourself. The most distinctive thing is that the agent logic lives entirely in an external MCP server, so the pipeline is a pure transport layer that can be swapped between Twilio, WebRTC, or a local simulator without changing the agent.

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
npm install @reaatech/create-voice-agent @reaatech/voice-agent-core @reaatech/voice-agent-mcp-client @reaatech/voice-agent-simulator @reaatech/voice-agent-stt @reaatech/voice-agent-telephony @reaatech/voice-agent-tts @reaatech/voice-agent-webrtc
```

| Package | Status | Purpose |
| --- | --- | --- |
| `@reaatech/create-voice-agent` | published v0.1.0 | A scaffolding CLI that generates a complete voice-agent-kit project with pipeline configuration, STT/TTS provider setup, telephony or WebRTC transport, an MCP client, and a read… |
| `@reaatech/voice-agent-core` | published v0.1.0 | A Zod-validated configuration system and pipeline orchestrator for building voice-enabled AI agents, providing a `createPipeline()` function that coordinates STT, MCP, and TTS s… |
| `@reaatech/voice-agent-mcp-client` | published v0.1.0 | A JSON-RPC 2.0 client that connects to any MCP server endpoint, providing tool discovery, conversation history management, retry with backoff, and TTS-safe response sanitization… |
| `@reaatech/voice-agent-simulator` | published v0.1.0 | A CLI and programmatic simulator that runs a voice agent pipeline (STT → MCP → TTS) locally from a WAV file or live microphone, reporting per-turn latency without requiring Twil… |
| `@reaatech/voice-agent-stt` | published v0.1.0 | Provider-agnostic speech-to-text interface with a unified `STTProvider` class and seven adapter implementations (Deepgram, AWS Transcribe, Google Cloud Speech-to-Text, OpenAI Re… |
| `@reaatech/voice-agent-telephony` | published v0.1.0 | A WebSocket handler for voice AI agents that normalizes bidirectional streaming protocols (start/media/stop/mark/DTMF) across Twilio, Telnyx, SignalWire, and Vonage, providing b… |
| `@reaatech/voice-agent-tts` | published v0.1.0 | Provider-agnostic text-to-speech interface with five adapter implementations (Deepgram Aura, AWS Polly, Google Cloud TTS, ElevenLabs, Cartesia), returning streaming audio as `As… |
| `@reaatech/voice-agent-webrtc` | published v0.1.0 | A WebSocket-based transport class (`WebRTCTransport`) for browser voice AI agents that handles Opus encode/decode, PCM resampling, and barge-in detection, paired with standalone… |

## Issue reporting

Failures while using this skill should be reported via the `report_issue` tool on the REAA SL MCP server (https://mcp.reaatech.com/sl, shipping in Phase 2). The skill name (`reaatech-voice-agent-kit`) is the canonical `package` identifier for that call.

## More

- Repo: https://github.com/reaatech/voice-agent-kit
- Browse packages: https://reaatech.com/products/domain-pipelines/voice-agent-kit/packages
- npm scope: https://www.npmjs.com/~reaatech
- tags: agentic-ai, calcom, conversational-ai, latency-optimization, rag, real-time, speech-to-text, stt, telephony, test-to-speech, tts, twillio, typescript, vector-search, voice-agent, voice-ai
