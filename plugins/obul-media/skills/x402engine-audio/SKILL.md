---
name: x402engine-audio
description: Text-to-speech and audio transcription via x402engine. Use when users want to generate speech audio from text or transcribe audio files to text.
---

# x402engine-audio Skill

Convert text to speech using OpenAI or ElevenLabs voices, or transcribe audio files to text with speaker diarization using Deepgram Nova-3.

## When to Use
- User wants to convert text to speech / generate audio from text
- User wants to transcribe an audio file to text
- User asks for TTS, text-to-speech, or voice generation
- User asks for speech-to-text, transcription, or audio-to-text
- User mentions ElevenLabs, OpenAI TTS, or Deepgram

## Endpoints

### Text-to-Speech (OpenAI)
- **URL**: `https://x402engine.app/api/tts/openai`
- **Method**: POST
- **Pricing**: $0.01 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"text": "Hello, this is a test of text-to-speech generation.", "voice": "alloy"}' \
  "https://x402engine.app/api/tts/openai" \
  -o output.mp3
```

**Voices:** `alloy`, `echo`, `fable`, `onyx`, `nova`, `shimmer`

**Response:** Returns audio data (MP3 or other format). Save to a file with `-o filename.mp3`.

---

### Text-to-Speech (ElevenLabs)
- **URL**: `https://x402engine.app/api/tts/elevenlabs`
- **Method**: POST
- **Pricing**: $0.02 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"text": "Welcome to the future of AI-generated speech.", "voice": "rachel"}' \
  "https://x402engine.app/api/tts/elevenlabs" \
  -o output.mp3
```

**Response:** Returns ultra-realistic audio data. ElevenLabs voices are more natural and expressive than OpenAI, but cost 2x more.

---

### Audio Transcription (Deepgram Nova-3)
- **URL**: `https://x402engine.app/api/transcribe`
- **Method**: POST
- **Pricing**: $0.10 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: multipart/form-data" \
  -F "file=@recording.mp3" \
  "https://x402engine.app/api/transcribe"
```

**Response:** JSON with transcribed text, speaker diarization labels, and timestamps.
```json
{
  "text": "Full transcription text here...",
  "segments": [
    {
      "speaker": "Speaker 1",
      "text": "Hello, how are you?",
      "start": 0.0,
      "end": 1.5
    }
  ]
}
```

## Notes
- Settlement is in USDC on Base chain (or USDm on MegaETH, USDC on Solana), handled automatically by the Obul proxy
- **OpenAI TTS** ($0.01) — reliable, 6 voices, HD quality, good for most use cases
- **ElevenLabs TTS** ($0.02) — ultra-realistic, multilingual, best voice quality
- **Transcription** ($0.10) — includes speaker diarization with labels and timestamps
- TTS endpoints return audio binary; always use `-o filename.mp3` to save output
- For transcription, send the audio file as multipart form data
