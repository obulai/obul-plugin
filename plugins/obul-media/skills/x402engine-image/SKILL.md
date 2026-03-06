---
name: x402engine-image
description: Generate images with FLUX models via x402engine. Use for fast image generation (FLUX Schnell) or production-quality output (FLUX.2 Pro), or text-in-image with Ideogram.
---

# x402engine-image Skill

Generate AI images using x402engine's pay-per-call endpoints. Choose between fast generation (FLUX Schnell, ~2s), high-quality output (FLUX.2 Pro), or text-in-image rendering (Ideogram v3).

## When to Use
- User wants a quick image generated fast (use `/api/image/fast`)
- User wants production-quality image output (use `/api/image/quality`)
- User needs text rendered inside an image, like logos or signage (use `/api/image/text`)
- User asks for FLUX, Schnell, or Ideogram specifically
- User wants the cheapest image generation option ($0.015)

## Endpoints

### Fast Image Generation (FLUX Schnell)
- **URL**: `https://x402engine.app/api/image/fast`
- **Method**: POST
- **Pricing**: $0.015 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "A serene mountain landscape at dawn with mist in the valleys"}' \
  "https://x402engine.app/api/image/fast"
```

**Response:** Returns the generated image data (PNG/JPEG). The response body contains the image binary or a JSON object with an image URL, depending on the endpoint version.

---

### Quality Image Generation (FLUX.2 Pro)
- **URL**: `https://x402engine.app/api/image/quality`
- **Method**: POST
- **Pricing**: $0.05 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "Professional product photo of a luxury watch on marble surface, studio lighting"}' \
  "https://x402engine.app/api/image/quality"
```

**Response:** Returns a higher-quality generated image. Slower than `/fast` but significantly better detail and coherence.

---

### Text-in-Image Generation (Ideogram v3)
- **URL**: `https://x402engine.app/api/image/text`
- **Method**: POST
- **Pricing**: $0.12 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"prompt": "A neon sign that reads OPEN 24 HOURS on a dark brick wall"}' \
  "https://x402engine.app/api/image/text"
```

**Response:** Returns an image with accurately rendered text. Best for logos, signs, and typography.

## Notes
- Settlement is in USDC on Base chain (or USDm on MegaETH, USDC on Solana), handled automatically by the Obul proxy
- **Fast** ($0.015) is ideal for prototyping and drafts — generates in ~2 seconds
- **Quality** ($0.05) is ideal for production use — significantly better detail
- **Text** ($0.12) is specialized for readable text in images — use only when text rendering is needed
- The primary request parameter is `prompt`; keep prompts descriptive for best results
- All endpoints return image data; save output to a file with `-o output.png` if needed
