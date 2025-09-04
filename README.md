# ACS Voice Agent

## Overview
AI-powered voice processing with real-time capabilities.

## Technology Stack
- **Real-time**: LiveKit
- **AI Models**: OpenAI, Cartesia, Whisper
- **Database**: PostgreSQL + Valkey
- **Integration**: Receives from Voice Hub

## Architecture
```
Voice Agent (3212)
    ├── LiveKit Server
    ├── OpenAI Integration
    ├── Cartesia TTS
    └── Whisper STT
```

## Context Keys
- `voice_agent.livekit_config` - LiveKit settings
- `voice_agent.ai_models` - Active AI models
- `voice_agent.active_sessions` - Live sessions
- `voice_agent.processing_queue` - Queue state
