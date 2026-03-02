---
name: obul-x402engine-audio
description: "USE THIS SKILL WHEN: the user wants to generate speech audio from text (TTS) or transcribe audio files to text. Provides pay-per-use text-to-speech and transcription via x402engine through the Obul proxy."
homepage: https://x402engine.app
metadata:
  obul-skill:
    emoji: "🔊"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# x402engine Audio

x402engine provides pay-per-call text-to-speech and audio transcription endpoints. Convert text to speech using OpenAI or ElevenLabs voices, or transcribe audio files to text with speaker diarization. No API key needed — payment is handled automatically by the Obul proxy.

## Authentication

All requests route through the Obul proxy. Include your Obul API key in every request:

```json
{
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

Base URL: `https://proxy.obul.ai/proxy/https/x402engine.app`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Text-to-Speech (OpenAI)

Generate speech audio from text using OpenAI's TTS models.

**Pricing:** $0.01

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/x402engine.app/api/tts/openai",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "text": "Hello, this is a test of text-to-speech generation.",
    "voice": "alloy"
  }
}
```

**Available voices:** `alloy`, `echo`, `fable`, `onyx`, `nova`, `shimmer`

**Response:** Returns audio data (MP3 or other format). Save to a file with `-o output.mp3`.

### Text-to-Speech (ElevenLabs)

Generate ultra-realistic speech using ElevenLabs voices.

**Pricing:** $0.02

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/x402engine.app/api/tts/elevenlabs",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "text": "Welcome to the future of AI-generated speech.",
    "voice": "rachel"
  }
}
```

**Response:** Returns ultra-realistic audio data. ElevenLabs voices are more natural and expressive but cost 2x more.

### Audio Transcription (Deepgram Nova-3)

Transcribe audio files to text with speaker diarization.

**Pricing:** $0.10

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/x402engine.app/api/transcribe",
  "headers": {
    "Content-Type": "multipart/form-data",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "file": "@recording.mp3"
  }
}
```

**Response:** JSON with transcribed text, speaker diarization labels, and timestamps.

## Endpoint Pricing Reference

| Endpoint                     | Price | Purpose                              |
|------------------------------|-------|--------------------------------------|
| `POST /api/tts/openai`      | $0.01 | Text-to-speech with OpenAI voices    |
| `POST /api/tts/elevenlabs`  | $0.02 | Text-to-speech with ElevenLabs voices |
| `POST /api/transcribe`      | $0.10 | Audio transcription with Deepgram   |

## When to Use

- **Text-to-speech** — User wants to convert text to speech or generate audio from text
- **Audio transcription** — User wants to transcribe an audio file to text
- **TTS specifically** — User asks for TTS, text-to-speech, or voice generation
- **Transcription** — User asks for speech-to-text, transcription, or audio-to-text
- **ElevenLabs/OpenAI** — User mentions ElevenLabs, OpenAI TTS, or Deepgram

## Best Practices

- **OpenAI TTS** — $0.01, reliable, 6 voices, HD quality, good for most use cases
- **ElevenLabs TTS** — $0.02, ultra-realistic, multilingual, best voice quality
- **Transcription** — $0.10 includes speaker diarization with labels and timestamps
- **Save TTS output** — TTS endpoints return audio binary; always use `-o filename.mp3` to save output
- **Multipart for transcription** — For transcription, send the audio file as multipart form data

## Error Handling

| Error                       | Cause                                    | Solution                                                                                  |
|-----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai.   |
| `400 Bad Request`           | Missing or invalid request body          | Ensure `text` is present for TTS or `file` is provided for transcription.                  |
| `415 Unsupported Media`     | Invalid audio format for transcription  | Ensure the audio file is in a supported format.                                            |
| `429 Too Many Requests`     | Rate limit exceeded                      | Add a short delay between requests.                                                       |
| `500 Internal Server Error` | x402engine service issue                 | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
