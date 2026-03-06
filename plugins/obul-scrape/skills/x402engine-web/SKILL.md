---
name: x402engine-web
description: "USE THIS SKILL WHEN: the user wants a quick single-page scrape to markdown or a webpage screenshot. Provides lightweight web scraping and screenshots via x402engine through the Obul proxy."
homepage: https://x402engine.app
metadata:
  obul-skill:
    emoji: "⚡"
registries: {}
---

# x402engine Web

x402engine provides pay-per-call web scraping and screenshot endpoints. Scrape any URL into clean markdown or capture a
full-page screenshot as a base64 image. Through the Obul proxy, each request is paid individually — no x402engine
account or API key required.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://x402-gateway-production.up.railway.app`

## Common Operations

### Web Scrape

Scrape a single URL and receive clean markdown content. No configuration needed — just pass a URL and get back
well-structured markdown extracted from the page.

**Pricing:** $0.005

```sh
obulx "https://x402-gateway-production.up.railway.app/api/web/scrape?url=https://example.com/page"
```

**Response:** JSON object with the scraped URL and clean markdown content extracted from the page.

### Web Screenshot

Capture a full-page screenshot of any URL. The page is rendered in a browser and returned as a base64-encoded PNG
image.

**Pricing:** $0.01

```sh
obulx "https://x402-gateway-production.up.railway.app/api/web/screenshot?url=https://example.com/page"
```

**Response:** JSON object with the URL and a base64-encoded PNG image of the rendered page. Decode the base64 string
and save as a `.png` file to view.

### Web Search

Perform a neural web search with highlighted snippets. Returns relevant results with context for a given query.

**Pricing:** $0.01

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"query": "x402 payment protocol"}' \
  "https://x402-gateway-production.up.railway.app/api/search/web"
```

**Response:** JSON object with search results, each containing title, URL, snippet, and relevance score.

### Extract Web Contents

Extract clean text content from up to 10 URLs in a single request. Useful for batch content extraction.

**Pricing:** $0.005

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"urls": ["https://example.com/page1", "https://example.com/page2"]}' \
  "https://x402-gateway-production.up.railway.app/api/search/contents"
```

**Response:** JSON object with extracted text content for each provided URL.

## Endpoint Pricing Reference

| Endpoint                   | Price  | Purpose                                       |
|----------------------------|--------|-----------------------------------------------|
| `GET /api/web/scrape`      | $0.005 | Scrape a URL to clean markdown                |
| `GET /api/web/screenshot`  | $0.01  | Capture a URL as a base64 PNG image           |
| `POST /api/search/web`     | $0.01  | Neural web search with snippets               |
| `POST /api/search/contents`| $0.005 | Extract clean text from up to 10 URLs         |

## When to Use

- **Quick single-page scrape** — Fastest and simplest way to get markdown from a URL with zero configuration.
- **Webpage screenshots** — Capture how a page looks in a browser for visual verification or archiving.
- **Batch content extraction** — Extract clean text from multiple URLs in a single request with `/api/search/contents`.
- **Budget-conscious scraping** — At $0.005/scrape and $0.01/screenshot, x402engine is one of the cheapest options.

## Best Practices

- **Use for simple pages** — x402engine web scrape works well for straightforward content pages. For anti-bot protected
  sites, use Zyte instead.
- **Save screenshots properly** — The screenshot response is a base64-encoded image. Decode it with
  `base64 -d > screenshot.png` to save as a viewable file.
- **Batch with contents endpoint** — When you have multiple URLs to extract, use `/api/search/contents` (up to 10 URLs)
  instead of calling `/api/web/scrape` repeatedly.
- **Prefer Firecrawl for complex scraping** — For multi-page crawls, structured extraction, or site mapping, Firecrawl
  is more capable.
- **Use scrape for LLM input** — The markdown output from `/api/web/scrape` is clean and token-efficient, ideal for
  feeding into LLM prompts or RAG pipelines.

## Error Handling

| Error                       | Cause                                    | Solution                                                                                 |
|-----------------------------|------------------------------------------|------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `400 Bad Request`           | Missing or invalid `url` parameter       | Ensure the `url` query parameter is present and is a valid URL.                          |
| `422 Unprocessable Entity`  | URL cannot be scraped or screenshotted   | The target URL may be inaccessible. Try a different URL.                                 |
| `429 Too Many Requests`     | Rate limit exceeded                      | Add a short delay between requests and avoid rapid-fire calls.                           |
| `500 Internal Server Error` | Upstream x402engine service issue        | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.   |
| `504 Gateway Timeout`       | Page took too long to render             | The target page may be very heavy. Try a simpler page or use basic HTTP scraping instead. |
