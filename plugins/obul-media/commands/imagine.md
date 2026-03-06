---
name: imagine
description: Generate an image from a text prompt. Usage: /obul-media:imagine <prompt>
---

Generate an AI image from a text prompt using the best available service for the request.

## Service Selection

Choose the service based on what the user needs:

| Need | Service | Endpoint | Cost |
|------|---------|----------|------|
| Quick draft or prototype | FLUX Schnell | `x402engine.app/api/image/fast` | $0.015 |
| Production-quality image | FLUX.2 Pro | `x402engine.app/api/image/quality` | $0.05 |
| Text/logos in image | Ideogram v3 | `x402engine.app/api/image/text` | $0.12 |
| High-res, stylized, or editorial | Freepik Mystic | `api.freepik.com/v1/x402/ai/mystic` | ~$0.02-0.05 |

### Decision Guide

1. **Default to FLUX Schnell** (`/api/image/fast`) for general requests — fastest and cheapest
2. **Use FLUX.2 Pro** (`/api/image/quality`) when the user says "high quality", "production", or "best quality"
3. **Use Ideogram** (`/api/image/text`) when the prompt contains text that needs to be readable in the image (signs, logos, labels)
4. **Use Freepik Mystic** when the user wants specific art styles (realism, fluid, zen, editorial), needs 4K resolution, or asks for Freepik specifically

## Workflow

1. Ensure you are logged in via `obulx login`
2. Select the best service based on the prompt and any user preferences
3. Send the request via obulx
4. Display the result — show the image URL or save the image file
5. Mention which service was used and the approximate cost

## Example

For a general prompt like "a cat sitting on a cloud":

```bash
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "a cat sitting on a cloud"}' \
  "https://x402engine.app/api/image/fast"
```

For a high-quality request:

```bash
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "a cat sitting on a cloud, photorealistic, studio lighting"}' \
  "https://x402engine.app/api/image/quality"
```

For a Freepik Mystic request with style controls:

```bash
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "a cat sitting on a cloud", "model": "realism", "resolution": "4k", "aspect_ratio": "widescreen_16_9"}' \
  "https://api.freepik.com/v1/x402/ai/mystic"
```
