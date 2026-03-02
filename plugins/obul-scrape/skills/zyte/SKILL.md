---
name: obul-zyte
description: "USE THIS SKILL WHEN: the user needs to scrape a website that blocks bots, requires JavaScript rendering, or needs anti-bot bypass. Provides heavy-duty web scraping via Zyte API through the Obul proxy."
homepage: https://www.zyte.com/zyte-api
metadata:
  obul-skill:
    emoji: "🛡️"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# Zyte

Zyte API provides pay-per-use web scraping with anti-bot protection bypass, full browser rendering, automatic CAPTCHA
solving, and AI-powered structured data extraction. Through the Obul proxy, each request is paid individually — no Zyte
account or API key required. Use Zyte when other scrapers fail due to bot detection.

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

Base URL: `https://proxy.obul.ai/proxy/https/api-x402.zyte.com`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Basic HTTP Scrape

Fetch the raw HTTP response body from a URL. Fastest and cheapest option — no browser rendering. The response body is
returned Base64-encoded.

**Pricing:** $0.08 per 1000 URLs (~$0.00008 per request)

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/api-x402.zyte.com/v1/extract",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://example.com/page",
    "httpResponseBody": true
  }
}
```

**Response:** JSON object with the URL, status code, and `httpResponseBody` as a Base64-encoded string. Decode the
Base64 to get the raw HTML content.

### Browser-Rendered Scrape

Render the page in a full browser with JavaScript execution. Use this for SPAs, dynamic pages, and sites that load
content via JavaScript.

**Pricing:** $0.28 per 1000 URLs (~$0.00028 per request)

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/api-x402.zyte.com/v1/extract",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://example.com/spa-page",
    "browserHtml": true
  }
}
```

**Response:** JSON object with the URL, status code, and `browserHtml` containing the fully rendered HTML including
JavaScript-generated content.

### Screenshot Capture

Capture a screenshot of the rendered page. Requires browser rendering. Useful for visual verification or archiving.

**Pricing:** $0.28 per 1000 URLs (~$0.00028 per request)

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/api-x402.zyte.com/v1/extract",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://example.com/page",
    "screenshot": true
  }
}
```

**Response:** JSON object with a Base64-encoded screenshot image of the rendered page.

### Product Data Extraction

Use Zyte's AI to automatically extract structured product data (name, price, currency, description, images) from
e-commerce pages.

**Pricing:** $0.76 per 1000 URLs (~$0.00076 per request)

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/api-x402.zyte.com/v1/extract",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://example.com/product/123",
    "product": true,
    "productOptions": {
      "extractFrom": "browserHtml"
    }
  }
}
```

**Response:** JSON object with a `product` field containing structured data: `name`, `price`, `currency`,
`description`, `images`, and other product attributes.

### Article Data Extraction

Use Zyte's AI to automatically extract structured article data (title, body, author, date) from news and blog pages.

**Pricing:** $0.76 per 1000 URLs (~$0.00076 per request)

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/api-x402.zyte.com/v1/extract",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://example.com/blog/article",
    "article": true
  }
}
```

**Response:** JSON object with an `article` field containing structured data: `headline`, `articleBody`, `author`,
`datePublished`, and other article attributes.

## Endpoint Pricing Reference

| Endpoint                | Price per 1000 URLs | Per-Request | Purpose                                        |
|-------------------------|---------------------|-------------|------------------------------------------------|
| `POST /v1/extract` (HTTP)       | $0.08               | ~$0.00008   | Raw HTTP response body (Base64)                |
| `POST /v1/extract` (browser)    | $0.28               | ~$0.00028   | Browser-rendered HTML or screenshot            |
| `POST /v1/extract` (product)    | $0.76               | ~$0.00076   | AI-powered product data extraction             |
| `POST /v1/extract` (article)    | $0.76               | ~$0.00076   | AI-powered article data extraction             |

## When to Use

- **Anti-bot bypass** — Target site uses Cloudflare, Akamai, DataDome, or other bot detection that blocks normal
  scrapers.
- **JavaScript rendering** — Page content loads via JavaScript (SPAs, React/Vue apps, infinite scroll).
- **Structured extraction** — Need product or article data extracted automatically by AI without writing custom parsers.
- **Geo-targeted scraping** — Need to scrape content as seen from a specific country or region.
- **Fallback scraper** — When Firecrawl or SimpleScraper fail on a URL, Zyte is the heavy-duty fallback.

## Best Practices

- **Start with httpResponseBody** — It is the cheapest option. Only use `browserHtml` if the page requires JavaScript
  rendering.
- **Use product/article extraction for structured data** — Zyte's AI extraction is more reliable than parsing HTML
  yourself for common page types.
- **Decode Base64 responses** — The `httpResponseBody` field is Base64-encoded. Always decode it before processing.
- **Use device emulation** — Set `"device": "mobile"` to see mobile-specific content or bypass desktop-only bot checks.
- **Combine features** — You can request both `browserHtml` and `screenshot` in a single request to get rendered content
  and a visual snapshot together.
- **Prefer cheaper scrapers first** — Zyte is the most powerful but also the most expensive. Use Firecrawl or
  SimpleScraper for sites that do not block scrapers.

## Error Handling

| Error                       | Cause                                    | Solution                                                                                 |
|-----------------------------|------------------------------------------|------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai. |
| `400 Bad Request`           | Missing or invalid request body          | Ensure `url` is present and at least one output flag is set (e.g., `httpResponseBody`).  |
| `422 Unprocessable Entity`  | URL cannot be scraped                    | The target URL may be down or completely blocking all access. Try a different approach.   |
| `429 Too Many Requests`     | Rate limit exceeded                      | Add a short delay between requests and avoid rapid-fire calls.                           |
| `500 Internal Server Error` | Upstream Zyte service issue              | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.   |
| `520 Anti-Bot Bypass Failed`| Bot detection could not be bypassed      | Try with `browserHtml` instead of `httpResponseBody`, or add a `geolocation` parameter.  |
