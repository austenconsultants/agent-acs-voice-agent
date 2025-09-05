# 🤖 ACS-Voice-Agent Architecture

## Component Overview

```
                    ┌────────────────────┐
                    │  ACS-Voice-Agent   │
                    │   AI Voice Tech    │
                    └─────────┬──────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
   ┌────▼─────┐         ┌────▼─────┐         ┌────▼─────┐
   │ LiveKit  │         │ Whisper  │         │  OpenAI  │
   │  WebRTC  │         │   STT    │         │   LLM    │
   └────┬─────┘         └────┬─────┘         └────┬─────┘
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              │
                    ┌─────────▼──────────┐
                    │    Cartesia TTS    │
                    └─────────┬──────────┘
                              │
                    ┌─────────▼──────────┐
                    │  Output to Voice   │
                    │  Platform (ACS)    │
                    └─────────────────────┘
```

## Connection Points

### Incoming Connections
- **From ACS-Manager**: AI configuration requests
- **From ACS-Voice**: Voice processing needs
- **From LiveKit**: WebRTC audio streams

### Outgoing Connections
- **To OpenAI API**: LLM processing
- **To Cartesia API**: Voice synthesis
- **To ACS-Voice**: Configuration updates
- **To Database**: Session storage

## Processing Pipeline

```python
# Real-time voice processing flow
async def process_voice_stream(audio_stream):
    # 1. Audio Input
    audio = await livekit.receive_audio(audio_stream)
    
    # 2. Speech to Text
    text = await whisper.transcribe(audio)
    
    # 3. LLM Processing
    response = await openai.complete(text)
    
    # 4. Text to Speech
    audio_response = await cartesia.synthesize(response)
    
    # 5. Stream Output
    await livekit.send_audio(audio_response)
```

## Database Schema

```sql
-- AI Model Configurations
ai_model_configs (
    model_name, provider,
    settings, api_key_ref
)

-- Voice Profiles
voice_profiles (
    profile_id, voice_name,
    tts_settings, personality
)

-- LiveKit Sessions
livekit_sessions (
    room_id, participant_id,
    start_time, metadata
)
```

## API Integration Points

```yaml
Services:
  LiveKit:
    URL: wss://livekit.cloud
    SDK: livekit-agents
    
  OpenAI:
    Model: gpt-4o-mini
    Endpoint: https://api.openai.com
    
  Whisper:
    Model: whisper-1
    Provider: OpenAI
    
  Cartesia:
    Voice: sonic-english
    Endpoint: https://api.cartesia.ai
```

---
*Part of AustenTel ACS Platform*
