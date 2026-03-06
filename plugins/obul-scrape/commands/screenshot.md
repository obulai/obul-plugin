---
description: Take a screenshot of a webpage via x402engine ($0.01/request)
---

# Screenshot

Take a screenshot of the given URL using x402engine.

**Cost:** $0.01 per request

## Usage

```
/obul-scrape:screenshot <URL>
/obul-scrape:screenshot <URL> --save <filename>
```

## Provider

This command uses the **x402engine-web** skill exclusively (`GET /api/web/screenshot` endpoint at $0.01/request).

## Workflow

1. Ensure you are logged in via `obulx login` (see setup rule)
2. Execute the screenshot request:
   ```bash
   obulx "https://x402-gateway-production.up.railway.app/api/web/screenshot?url=<TARGET_URL>"
   ```
3. The response contains a base64-encoded image in JSON format
4. Decode and save the image to a file:
   ```bash
   obulx "https://x402-gateway-production.up.railway.app/api/web/screenshot?url=<TARGET_URL>" \
     | jq -r '.image' | base64 -d > screenshot.png
   ```
5. If the user specified `--save <filename>`, save to that filename; otherwise use `screenshot.png`
6. Confirm the file was saved and its size
7. If the screenshot fails, suggest the user try with **Zyte** (`zyte` skill with `"screenshot": true`) as a fallback for anti-bot protected sites
