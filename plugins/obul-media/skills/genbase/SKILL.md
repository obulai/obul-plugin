---
name: obul-genbase
description: "USE THIS SKILL WHEN: the user wants to generate AI videos or images. Provides pay-per-use video generation with OpenAI Sora 2, xAI Grok Imagine, and GPT Image via Genbase through the Obul proxy."
homepage: https://www.genbase.fun
metadata:
  obul-skill:
    emoji: "🎬"
registries: {}
---

# Genbase AI Video Generation

Generate AI videos and images via Genbase's x402-enabled platform. Supports multiple generation models including
OpenAI Sora 2 for high-quality video, xAI Grok Imagine for fast and flexible video, and GPT Image for AI image
generation. Through the Obul proxy, each generation is paid individually — no Genbase account or API key required.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://www.genbase.fun`

## Common Operations

### Generate Video with Sora 2

Generate high-quality AI video using OpenAI's Sora 2 model. Supports multiple durations and resolutions including
landscape, portrait, wide, and tall formats. Generation takes 3-15 minutes depending on the model tier.

**Pricing:** $0.20

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "A drone shot flying over a misty mountain valley at sunrise, cinematic lighting", "model": "sora-2", "seconds": "10", "size": "1280x720"}' \
  "https://www.genbase.fun/api/video/create-sora2"
```

**Parameters:**

| Parameter | Type   | Required | Options                                                   | Description                        |
|-----------|--------|----------|-----------------------------------------------------------|------------------------------------|
| `prompt`  | string | yes      | —                                                         | Video description                  |
| `model`   | string | no       | `sora-2`, `sora-2-pro`                                    | Model tier (pro supports 25s)      |
| `seconds` | string | no       | `10`, `15`, `25` (pro only)                               | Video duration                     |
| `size`    | string | no       | `1280x720`, `720x1280`, `1792x1024`, `1024x1792`         | Output resolution                  |

**Response:**
```json
{
  "video_id": "task-uuid",
  "status": "pending",
  "model": "sora-2",
  "seconds": "10",
  "size": "1280x720"
}
```

### Generate Video with xAI Grok Imagine

Generate AI video using xAI's Grok Imagine model. Supports text-to-video, image-to-video, and video-to-video modes
with flexible duration (1-15 seconds) and multiple aspect ratios. Generation takes 2-5 minutes.

**Pricing:** $0.01 per second of video (1-15 seconds)

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "A cat playing piano in a jazz club, warm ambient lighting", "duration": 10, "aspectRatio": "16:9", "resolution": "720p", "mode": "text-to-video"}' \
  "https://www.genbase.fun/api/video/create-xai"
```

**Parameters:**

| Parameter        | Type    | Required | Options                                                        | Description                          |
|------------------|---------|----------|----------------------------------------------------------------|--------------------------------------|
| `prompt`         | string  | yes      | —                                                              | Video description                    |
| `duration`       | integer | no       | 1-15                                                           | Video length in seconds              |
| `aspectRatio`    | string  | no       | `16:9`, `9:16`, `1:1`, `4:3`, `3:4`                           | Output aspect ratio                  |
| `resolution`     | string  | no       | `480p`, `720p`                                                 | Output resolution                    |
| `mode`           | string  | no       | `text-to-video`, `image-to-video`, `video-to-video`            | Generation mode                      |
| `referenceImage` | string  | no       | —                                                              | URL for image/video reference modes  |

**Response:**
```json
{
  "request_id": "req-uuid",
  "status": "pending",
  "provider": "xai",
  "duration": 10
}
```

### Generate Image with GPT Image

Generate an AI image using GPT Image. Supports text-to-image and image-to-image with an optional base64-encoded
reference image. Generation takes 30-90 seconds.

**Pricing:** $0.02

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "A minimalist logo design for a tech startup, clean lines, blue and white color scheme"}' \
  "https://www.genbase.fun/api/image/create"
```

**Parameters:**

| Parameter        | Type   | Required | Description                                   |
|------------------|--------|----------|-----------------------------------------------|
| `prompt`         | string | yes      | Image description                             |
| `referenceImage` | string | no       | Base64-encoded image for image-to-image mode  |

**Response:**
```json
{
  "image_id": "img-uuid",
  "status": "completed",
  "url": "https://..."
}
```

### Query Video Status (Sora 2)

Poll the status of a Sora 2 video generation task. Free endpoint — no payment required.

**Pricing:** $0.00

```sh
obulx "https://www.genbase.fun/api/video/query?id={video_id}"
```

**Response:**
```json
{
  "video_id": "task-uuid",
  "status": "Ready",
  "progress": 100,
  "url": "https://...video_url..."
}
```

**Status values:** `pending` (20%), `processing` (50%), `video_upsampling` (80%), `Ready` (100%), `failed`

### Query Video Status (xAI)

Poll the status of an xAI Grok Imagine video generation task. Free endpoint — no payment required.

**Pricing:** $0.00

```sh
obulx "https://www.genbase.fun/api/video/query-xai?id={request_id}"
```

**Response:**
```json
{
  "request_id": "req-uuid",
  "status": "completed",
  "url": "https://...video_url..."
}
```

**Status values:** `pending`, `completed`, `failed`

## Endpoint Pricing Reference

| Endpoint                       | Price              | Purpose                                        |
|--------------------------------|--------------------|------------------------------------------------|
| `POST /api/video/create-sora2` | $0.20              | Generate video with OpenAI Sora 2              |
| `POST /api/video/create-xai`   | $0.01/sec          | Generate video with xAI Grok Imagine           |
| `POST /api/image/create`       | $0.02              | Generate image with GPT Image                  |
| `GET /api/video/query`         | $0.00              | Poll Sora 2 generation status                  |
| `GET /api/video/query-xai`     | $0.00              | Poll xAI generation status                     |
| `GET /api/image/query`         | $0.00              | Poll image generation status                   |

## When to Use

- **High-quality video** — Use Sora 2 for cinematic, high-fidelity video generation when quality matters most
  ($0.20 per video).
- **Fast and flexible video** — Use xAI Grok Imagine for quicker generation with flexible duration (1-15 seconds) and
  multiple input modes at $0.01/sec.
- **Image-to-video** — Use xAI with `mode: "image-to-video"` and a reference image URL to animate a still image.
- **Quick image generation** — Use GPT Image for fast, affordable AI image generation at $0.02 per image.
- **Budget-conscious video** — xAI at $0.01/sec for a 5-second video ($0.05) is significantly cheaper than Sora 2
  ($0.20) when maximum quality is not required.

## Best Practices

- **Poll for completion** — Video generation is asynchronous. After submitting a request, poll the corresponding query
  endpoint every 15-30 seconds until the status is `Ready` or `completed`.
- **Use Sora 2 for quality, xAI for speed** — Sora 2 produces higher-quality output but takes 3-15 minutes. xAI
  generates in 2-5 minutes and is cheaper for shorter clips.
- **Start with short durations** — Use 10-second Sora 2 videos or short xAI clips to validate your prompt before
  generating longer content.
- **Write cinematic prompts** — Include camera movements (drone shot, tracking shot), lighting (golden hour,
  cinematic), and specific visual details for the best results.
- **Mind content policy** — Sora 2 enforces OpenAI's content policy. Prompts with violence or adult content will cause
  generation failure. Contact contact@genbase.fun with video_id and wallet address for refunds on policy failures.
- **Choose the right resolution** — For Sora 2, use `1280x720` for landscape, `720x1280` for portrait/vertical
  content, `1792x1024` for widescreen, and `1024x1792` for tall formats.

## Error Handling

| Error                          | Cause                                        | Solution                                                                                  |
|--------------------------------|----------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`         | Payment not processed or insufficient        | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `400 Bad Request`              | Missing or invalid request body              | Ensure `prompt` is present. Check that `model`, `seconds`, and `size` are valid values.   |
| `422 Content Policy Violation` | Prompt violates content policy               | Revise prompt to comply with guidelines. Contact support for refund if charged.            |
| `404 Not Found`                | Invalid video/image ID in query              | Verify the ID matches the one returned from the creation endpoint.                        |
| `429 Too Many Requests`        | Rate limit exceeded                          | Add delays between requests. Video generation is resource-intensive.                      |
| `500 Internal Server Error`    | Upstream Genbase service issue               | Wait and retry. If persistent, the service may be experiencing downtime.                  |
| `failed` status                | Generation failed (model or content issue)   | Check prompt for content policy violations. Try a simpler prompt or different model.      |
