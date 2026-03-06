---
name: obul-dtelecom
description: "USE THIS SKILL WHEN: the user wants to transcribe audio or speech to text in real-time. Provides production-grade speech-to-text with dual-engine architecture, 99+ languages, VAD, noise reduction, and hallucination filtering via dTelecom through the Obul proxy."
homepage: https://x402stt.dtelecom.org
metadata:
  obul-skill:
    emoji: "🎙️"
registries: {}
---

# dTelecom Speech-to-Text

Production-grade real-time speech-to-text for AI agents via dTelecom's x402-enabled STT service. Features a
dual-engine architecture (Parakeet-TDT for 25 European languages at 3-4x speed, Whisper for 99+ languages) with
smart routing, voice activity detection, neural noise reduction, speech validation, and hallucination filtering.
Through the Obul proxy, each session is paid per minute at $0.005/min — no dTelecom account or API key required.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://x402stt.dtelecom.org`

## Common Operations

### Create a Transcription Session

Create and pay for a new STT session. Specify the number of minutes and target language. The response includes a
session key and WebSocket URL for streaming audio.

**Pricing:** $0.005 per minute (minimum 5 minutes = $0.025, maximum 120 minutes = $0.60)

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"minutes": 5, "language": "en"}' \
  "https://x402stt.dtelecom.org/v1/session"
```

**Response:**
```json
{
  "session_id": "abc-123-uuid",
  "session_key": "eyJ...",
  "ws_url": "wss://x402stt.dtelecom.org/v1/stream",
  "remaining_seconds": 300,
  "minutes": 5,
  "price_usd": "0.025000"
}
```

### Extend an Active Session

Add more time to an active session without interrupting the audio stream.

**Pricing:** $0.005 per minute added

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"session_id": "abc-123-uuid", "minutes": 5}' \
  "https://x402stt.dtelecom.org/v1/session/extend"
```

**Response:**
```json
{
  "session_id": "abc-123-uuid",
  "remaining_seconds": 600,
  "minutes_added": 5,
  "price_usd": "0.025000"
}
```

### Check Session Status

Check the remaining time and usage of an active session. This endpoint is free.

**Pricing:** $0.00

```sh
obulx "https://x402stt.dtelecom.org/v1/session/{session_id}/status"
```

**Response:**
```json
{
  "session_id": "abc-123-uuid",
  "status": "connected",
  "remaining_seconds": 245.3,
  "used_seconds": 54.7,
  "balance_seconds": 300,
  "language": "en"
}
```

### Stream Audio via WebSocket

After creating a session, connect to the WebSocket URL with the session key. Send audio as PCM16 (16-bit
little-endian, 16kHz, mono) in 20ms chunks (640 bytes). The server returns real-time transcription results.

**Pricing:** Included in session cost

**WebSocket handshake (client sends):**
```json
{
  "type": "config",
  "language": "en",
  "session_key": "eyJ..."
}
```

**Server ready response:**
```json
{
  "type": "ready",
  "remaining_seconds": 300
}
```

**Transcription response (server sends for each utterance):**
```json
{
  "type": "transcription",
  "text": "Hello, how are you?",
  "start": 1.5,
  "end": 2.8,
  "confidence": 0.95,
  "is_final": true
}
```

### Check Service Health

Verify the STT service is available. No payment required.

**Pricing:** $0.00

```sh
obulx "https://x402stt.dtelecom.org/health"
```

## Endpoint Pricing Reference

| Endpoint                          | Price             | Purpose                                      |
|-----------------------------------|-------------------|----------------------------------------------|
| `POST /v1/session`                | $0.005/min        | Create and pay for a new STT session         |
| `POST /v1/session/extend`         | $0.005/min        | Add time to an active session                |
| `GET /v1/session/{id}/status`     | $0.00             | Check remaining session time and usage       |
| `WS /v1/stream`                   | Included          | Stream audio and receive transcriptions      |
| `GET /health`                     | $0.00             | Service availability check                   |
| `GET /pricing`                    | $0.00             | Retrieve current rate information            |

## When to Use

- **Real-time transcription** — Stream audio and get live transcription results with confidence scores and timestamps.
- **Multi-language support** — Transcribe in 99+ languages with automatic engine selection (Parakeet-TDT for European
  languages, Whisper for others).
- **Long-running sessions** — Record meetings, interviews, or calls with sessions up to 120 minutes, extendable
  without interruption.
- **Cost-effective transcription** — At $0.005/min, dTelecom is 20x cheaper than x402engine transcription ($0.10/req)
  for audio longer than a few seconds.
- **Noisy audio** — Built-in VAD, noise reduction, and hallucination filtering handle real-world audio conditions.

## Best Practices

- **Start with 5-minute sessions** — Use the minimum session length and extend as needed rather than over-purchasing
  time upfront.
- **Use the correct audio format** — Send PCM16, 16kHz, mono audio in 20ms chunks (640 bytes). Incorrect formats will
  produce garbled results or errors.
- **Set the language parameter** — While both engines support auto-detection, specifying the language improves accuracy
  and routes to the optimal engine.
- **Monitor session expiry warnings** — The server sends `session_expiring` messages at 60 and 10 seconds remaining.
  Use these to trigger automatic session extension.
- **Handle reconnection** — If the WebSocket disconnects, the billing clock pauses. Reconnect with the same session
  key to resume without losing paid time.
- **Prefer Parakeet-TDT languages** — For English and other European languages, the Parakeet-TDT engine is 3-4x
  faster with 3-5% word error rate.

## Error Handling

| Error                          | Cause                                        | Solution                                                                                  |
|--------------------------------|----------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`         | x402 payment not processed                   | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `400 Bad Request`              | Missing or invalid parameters                | Ensure `minutes` (5-120) and `language` are provided and correctly typed.                  |
| `404 Not Found`                | Session ID does not exist                    | Verify the session ID is correct. Sessions expire after their purchased time runs out.    |
| `WS 4001`                      | Config timeout or invalid config             | Send the config message with valid session_key immediately after WebSocket connection.    |
| `WS 4002`                      | Session expired or not found                 | Create a new session. The previous session's time has been exhausted.                     |
| `WS 4003`                      | Authentication failure                       | Verify the session_key matches the one returned from session creation.                    |
| `WS 4004`                      | Session already connected elsewhere          | Only one WebSocket connection per session is allowed. Close the other connection first.   |
| `500 Internal Server Error`    | Upstream dTelecom service issue              | Wait a few seconds and retry. If persistent, check `/health` for service status.          |
