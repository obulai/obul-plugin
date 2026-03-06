---
name: obul-freepik
description: "USE THIS SKILL WHEN: the user wants to generate high-quality AI images with realistic, stylized, or editorial styles. Provides pay-per-use AI image generation via Freepik Mystic through the Obul proxy."
homepage: https://www.freepik.com
metadata:
  obul-skill:
    emoji: "🎨"
    requires:
      env: []
      primaryEnv: ""
registries: {}
---

# Freepik

Freepik provides high-quality AI image generation through the Mystic model. Supports multiple art styles, resolutions up to 4K, and advanced controls like style and structure references. No API key needed — payment is handled automatically via `obulx`.

## Authentication

All requests use the `obulx` CLI, which handles x402 payment automatically.

## Common Operations

### Mystic Image Generation

Generate an AI image using the Mystic model with customizable style, resolution, and aspect ratio.

**Pricing:** $0.02

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "A futuristic city at sunset with neon lights reflecting on wet streets", "model": "realism", "resolution": "2k", "aspect_ratio": "widescreen_16_9"}' \
  "https://api.freepik.com/v1/x402/ai/mystic"
```

**Parameters:**

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `prompt` | string | yes | — | Text description of the image to generate |
| `model` | enum | no | `realism` | `realism`, `fluid`, `zen`, `flexible`, `super_real`, `editorial_portraits` |
| `resolution` | enum | no | `2k` | `1k`, `2k`, `4k` |
| `aspect_ratio` | enum | no | `square_1_1` | `square_1_1`, `widescreen_16_9`, `social_story_9_16`, `traditional_3_4` |
| `hdr` | int | no | `50` | 0-100, detail level vs. natural appearance |
| `creative_detailing` | int | no | `33` | 0-100, detail intensity per pixel |
| `engine` | enum | no | `automatic` | `automatic`, `magnific_illusio`, `magnific_sharpy`, `magnific_sparkle` |
| `fixed_generation` | bool | no | `false` | Reproducible results with identical settings |
| `filter_nsfw` | bool | no | `true` | NSFW content filtering |
| `structure_reference` | string | no | — | Base64-encoded image for structural guidance |
| `structure_strength` | int | no | `50` | 0-100, how closely to follow structure reference |
| `style_reference` | string | no | — | Base64-encoded image for aesthetic influence |
| `adherence` | int | no | `50` | 0-100, balance between prompt fidelity and style transfer |

**Response:** JSON with task status and generated image URLs.

### Higher Quality Generation

Generate with higher detail and quality settings.

**Pricing:** $0.05

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "Professional product photo of a luxury watch on marble surface, studio lighting", "model": "super_real", "resolution": "4k", "hdr": 75, "creative_detailing": 50}' \
  "https://api.freepik.com/v1/x402/ai/mystic"
```

**Response:** JSON with task status and generated image URLs.

## Endpoint Pricing Reference

| Endpoint                    | Price   | Purpose                              |
|-----------------------------|---------|--------------------------------------|
| `POST /v1/x402/ai/mystic`  | $0.02-0.05 | AI image generation with Mystic    |

## When to Use

- **AI image generation** — User wants a high-quality, realistic AI-generated image
- **Art styles** — User requests specific art styles (realism, fluid, zen, editorial portraits)
- **High resolution** — User needs high-resolution output (2K or 4K)
- **Fine control** — User wants fine control over aspect ratio, style references, or HDR
- **Freepik specifically** — User asks for Freepik or Mystic specifically

## Best Practices

- **Model selection** — The `model` parameter significantly changes output style: `realism` for photorealistic, `fluid` for artistic, `zen` for minimalist, `super_real` for hyper-realistic
- **Style references** — Style references and LoRAs are ignored when using `fluid`, `flexible`, `super_real`, or `editorial_portraits` models
- **Write detailed prompts** — For best results, write detailed prompts describing subject, style, lighting, and composition
- **Resolution options** — Available: `1k`, `2k`, `4k`
- **Aspect ratios** — Available: `square_1_1`, `widescreen_16_9`, `social_story_9_16`, `traditional_3_4`

## Error Handling

| Error                       | Cause                                    | Solution                                                                                  |
|-----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your obulx setup is correct and your account has sufficient balance at my.obul.ai.  |
| `400 Bad Request`           | Invalid request body                    | Ensure required fields like `prompt` are present and correctly formatted.                 |
| `422 Unprocessable Entity`  | Prompt violates content policy          | Revise the prompt to comply with content guidelines.                                       |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests.                                                       |
| `500 Internal Server Error` | Freepik service issue                    | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
