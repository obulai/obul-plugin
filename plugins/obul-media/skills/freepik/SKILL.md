---
name: freepik
description: Generate high-quality AI images with Freepik Mystic. Use when users want realistic, stylized, or editorial AI images with fine-grained control over resolution, aspect ratio, and style.
---

# freepik Skill

Generate AI images using Freepik's Mystic model via their x402-enabled endpoint. Supports multiple art styles, resolutions up to 4K, and advanced controls like style/structure references.

## When to Use
- User wants a high-quality, realistic AI-generated image
- User requests specific art styles (realism, fluid, zen, editorial portraits)
- User needs high-resolution output (2K or 4K)
- User wants fine control over aspect ratio, style references, or HDR
- User asks for Freepik or Mystic specifically

## Endpoints

### Mystic Image Generation
- **URL**: `https://api.freepik.com/v1/x402/ai/mystic`
- **Method**: POST
- **Pricing**: ~$0.02-0.05 per request

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
```json
{
  "data": {
    "task_id": "uuid",
    "status": "COMPLETED",
    "generated": ["https://...image_url..."]
  }
}
```

If `status` is `IN_PROGRESS`, poll the task or use a webhook. In practice, images typically return inline.

## Notes
- Settlement is in USDC on Base chain, handled automatically by the Obul proxy
- The `model` parameter significantly changes output style: `realism` for photorealistic, `fluid` for artistic, `zen` for minimalist, `super_real` for hyper-realistic
- Style references and LoRAs are ignored when using `fluid`, `flexible`, `super_real`, or `editorial_portraits` models
- For best results, write detailed prompts describing subject, style, lighting, and composition
