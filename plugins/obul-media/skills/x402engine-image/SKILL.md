---
name: obul-x402engine-image
description: "USE THIS SKILL WHEN: the user wants to generate AI images with FLUX models or create text-in-image with Ideogram. Provides pay-per-use image generation via x402engine through the Obul proxy."
homepage: https://x402engine.app
metadata:
  obul-skill:
    emoji: "🖼️"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# x402engine Image

x402engine provides pay-per-call AI image generation endpoints. Choose between fast generation (FLUX Schnell), high-quality output (FLUX.2 Pro), or text-in-image rendering (Ideogram v3). No API key needed — payment is handled automatically by the Obul proxy.

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

### Fast Image Generation (FLUX Schnell)

Generate an image quickly for prototyping and drafts.

**Pricing:** $0.015

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/x402engine.app/api/image/fast",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "prompt": "A serene mountain landscape at dawn with mist in the valleys"
  }
}
```

**Response:** Returns the generated image data (PNG/JPEG).

### Quality Image Generation (FLUX.2 Pro)

Generate a higher-quality image for production use.

**Pricing:** $0.05

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/x402engine.app/api/image/quality",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "prompt": "Professional product photo of a luxury watch on marble surface, studio lighting"
  }
}
```

**Response:** Returns a higher-quality generated image with better detail and coherence.

### Text-in-Image Generation (Ideogram v3)

Generate an image with accurately rendered text, ideal for logos, signs, and typography.

**Pricing:** $0.12

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/x402engine.app/api/image/text",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "prompt": "A neon sign that reads OPEN 24 HOURS on a dark brick wall"
  }
}
```

**Response:** Returns an image with accurately rendered text.

## Endpoint Pricing Reference

| Endpoint                | Price  | Purpose                              |
|------------------------|--------|--------------------------------------|
| `POST /api/image/fast` | $0.015 | Fast FLUX Schnell generation         |
| `POST /api/image/quality` | $0.05 | High-quality FLUX.2 Pro generation |
| `POST /api/image/text` | $0.12  | Text-in-image with Ideogram         |

## When to Use

- **Quick generation** — User wants a fast image generated for prototyping (use `/api/image/fast`)
- **Quality output** — User wants production-quality image output (use `/api/image/quality`)
- **Text in image** — User needs text rendered inside an image, like logos or signage (use `/api/image/text`)
- **FLUX specifically** — User asks for FLUX, Schnell, or Ideogram specifically
- **Budget-conscious** — User wants the cheapest image generation option

## Best Practices

- **Fast for drafts** — The fast endpoint ($0.015) is ideal for prototyping and drafts — generates in ~2 seconds
- **Quality for production** — The quality endpoint ($0.05) is ideal for production use with significantly better detail
- **Text specialized** — The text endpoint ($0.12) is specialized for readable text in images — use only when text rendering is needed
- **Descriptive prompts** — Keep prompts descriptive for best results
- **Save output** — All endpoints return image data; save output to a file with `-o output.png`

## Error Handling

| Error                       | Cause                                    | Solution                                                                                  |
|-----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai.   |
| `400 Bad Request`           | Missing or invalid request body          | Ensure `prompt` is present and is a non-empty string.                                      |
| `422 Unprocessable Entity`  | Prompt violates content policy           | Revise the prompt to comply with content guidelines.                                       |
| `429 Too Many Requests`     | Rate limit exceeded                      | Add a short delay between requests.                                                       |
| `500 Internal Server Error` | x402engine service issue                 | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
| `504 Gateway Timeout`       | Image generation took too long           | Try the fast endpoint or simplify the prompt.                                              |
