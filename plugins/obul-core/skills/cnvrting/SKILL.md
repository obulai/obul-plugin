---
name: obul-cnvrting
description: "USE THIS SKILL WHEN: the user wants to convert media files, transcribe audio or video, extract text from images via OCR, or download and convert content from YouTube, TikTok, Instagram, or other platforms. Provides pay-per-use media conversion and transcription via cnvrt.ing through the Obul proxy."
homepage: https://cnvrt.ing
metadata:
  obul-skill:
    emoji: "🔄"
registries: {}
---

# cnvrt.ing

cnvrt.ing is a universal media conversion and analysis service — convert, transcribe, and analyze media from over 1000
platforms including YouTube, TikTok, Instagram, Twitter, and more. Through the Obul proxy, each request is paid
individually via x402 — no account or API key required. Supports audio, video, and image formats with AI-powered
transcription and image analysis.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://cnvrt.ing`

## Common Operations

### Convert Media

Convert media from a URL to a different format. Supports 1000+ platforms including YouTube, TikTok, Instagram, Twitter,
Facebook, Vimeo, Twitch, SoundCloud, Reddit, and more. Output formats include mp3, mp4, wav, aac, flac, ogg, m4a, mov,
mkv, avi, webm, jpg, png, webp, gif, and bmp.

**Pricing:** $0.00 (free)

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"url": "https://www.youtube.com/watch?v=VIDEO_ID", "format": "mp3", "quality": "best"}' \
  "https://cnvrt.ing/api/convert"
```

**Response:** JSON object with the converted media file URL. Download the file from the returned URL.

### Transcribe Audio or Video

Transcribe audio or video content to text using OpenAI Whisper with 95%+ accuracy. Provide a URL to any supported
platform and receive a full text transcription.

**Pricing:** $0.025

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"url": "https://www.youtube.com/watch?v=VIDEO_ID"}' \
  "https://cnvrt.ing/api/transcribe"
```

**Response:** JSON object with the transcribed text content. Includes the full transcription of the audio or video.

### Analyze Image (OCR and Object Detection)

Extract text via OCR and detect objects in images using GPT-4o Vision. Provide a URL to an image and receive structured
analysis results.

**Pricing:** $0.005

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"url": "https://example.com/image.png"}' \
  "https://cnvrt.ing/api/analyze-image"
```

**Response:** JSON object containing extracted text (OCR) and detected objects from the image.

### Detect Format

Identify the format and metadata of a media URL before converting. Useful for determining what formats are available
and estimating conversion costs.

**Pricing:** $0.00 (free)

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"url": "https://www.youtube.com/watch?v=VIDEO_ID"}' \
  "https://cnvrt.ing/api/detect-format"
```

**Response:** JSON object with the detected format, resolution, duration, and other metadata about the media.

### Estimate Cost

Pre-flight cost estimation for paid operations like transcription and image analysis. Check the price before committing
to a paid request.

**Pricing:** $0.00 (free)

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"url": "https://www.youtube.com/watch?v=VIDEO_ID", "operation": "transcribe"}' \
  "https://cnvrt.ing/api/estimate-cost"
```

**Response:** JSON object with the estimated cost in USDC for the requested operation.

## Endpoint Pricing Reference

| Endpoint                    | Price   | Purpose                                              |
|-----------------------------|---------|------------------------------------------------------|
| `POST /api/convert`        | $0.00   | Convert media from 1000+ platforms to any format     |
| `POST /api/transcribe`     | $0.025  | Transcribe audio/video to text via OpenAI Whisper    |
| `POST /api/analyze-image`  | $0.005  | OCR and object detection via GPT-4o Vision           |
| `POST /api/detect-format`  | $0.00   | Detect format and metadata of a media URL            |
| `POST /api/estimate-cost`  | $0.00   | Estimate cost for a paid operation                   |
| `POST /api/convertlive`    | $0.05   | Real-time live stream analysis with transcription    |
| `POST /api/validate`       | $0.00   | Pre-flight URL validation                            |

## When to Use

- **Media downloading** — Download and convert videos or audio from YouTube, TikTok, Instagram, Twitter, and 1000+
  other platforms to common formats like mp3 or mp4.
- **Transcription** — Convert spoken content in videos or audio files to text with high accuracy, useful for meeting
  notes, podcast transcripts, or content indexing.
- **Image analysis** — Extract text from images via OCR or detect objects in photographs, screenshots, or documents.
- **Format conversion** — Convert between audio formats (mp3, wav, flac, aac), video formats (mp4, mkv, webm), or
  image formats (jpg, png, webp).
- **Content research** — Download and transcribe video content for research, summarization, or knowledge base building.

## Best Practices

- **Use detect-format first** — Call `/api/detect-format` before converting to understand the source media's properties
  and available quality levels.
- **Use estimate-cost for paid operations** — Call `/api/estimate-cost` before transcription or image analysis to check
  pricing, especially for long media files.
- **Choose the right quality** — Set `"quality": "best"` for highest quality output, or omit for default quality which
  balances size and fidelity.
- **Use convert for free downloads** — The `/api/convert` endpoint is free, making it ideal for downloading media in
  common formats without cost.
- **Prefer transcribe over manual extraction** — For audio or video content, use the transcription endpoint rather than
  downloading and processing locally. The Whisper-based transcription is fast and accurate.

## Error Handling

| Error                       | Cause                                    | Solution                                                                                   |
|-----------------------------|------------------------------------------|--------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `400 Bad Request`           | Missing or invalid request body          | Ensure `url` is provided and is a valid, accessible URL. Check `format` is supported.      |
| `404 Not Found`             | URL content not found on platform        | Verify the URL is correct and the content is still available on the source platform.       |
| `415 Unsupported Media`     | Format not supported for conversion      | Check supported formats. Use `/api/detect-format` to see available options.                |
| `429 Too Many Requests`     | Rate limit exceeded                      | Add a short delay between requests. Avoid rapid-fire conversion calls.                     |
| `500 Internal Server Error` | Upstream service issue                   | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
| `504 Gateway Timeout`       | Media processing took too long           | The source file may be too large. Try a shorter clip or lower quality setting.              |
