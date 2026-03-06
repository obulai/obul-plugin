---
name: obul-minifetch
description: "USE THIS SKILL WHEN: the user needs URL metadata, Open Graph tags, SEO data, or link discovery from a web page. Provides lightweight metadata extraction via Minifetch through the Obul proxy."
homepage: https://minifetch.com
metadata:
  obul-skill:
    emoji: "📋"
registries: {}
---

# Minifetch

Minifetch provides pay-per-use URL metadata extraction and link discovery. It returns rich structured metadata
including Open Graph tags, Twitter Cards, JSON-LD, hreflang, favicons, and all links from any web page. Through the
Obul proxy, each request is paid individually — no Minifetch account or API key required.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://minifetch.com`

## Common Operations

### Extract URL Metadata

Extract rich metadata from a URL including title, description, Open Graph tags, Twitter Cards, JSON-LD structured data,
hreflang tags, favicons, and robots directives.

**Pricing:** $0.002

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"url": "https://example.com/page"}' \
  "https://minifetch.com/api/v1/x402/extract/url-metadata"
```

**Response:** JSON object with extracted metadata: `title`, `description`, `image`, `url`, `lang`, `favicons`,
`ogTitle`, `ogDescription`, `ogImage`, `twitterCard`, `twitterTitle`, `jsonLd`, `hreflang`, `robots`, and
`statusCode`.

### Extract URL Metadata (Full Verbosity)

Same as above but with full verbosity and optional HTML response body included. Use this when you need every available
metadata field and the raw page content.

**Pricing:** $0.002

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"url": "https://example.com/page", "verbosity": "full", "includeResponseBody": true}' \
  "https://minifetch.com/api/v1/x402/extract/url-metadata"
```

**Response:** JSON object with all metadata fields plus the full HTML response body. The `verbosity: "full"` flag
includes every available tag and attribute found on the page.

### Extract URL Links

Discover all links on a page. Returns a flat list of all URLs found in the page HTML — useful for site mapping, SEO
link analysis, and crawl planning.

**Pricing:** $0.002

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"url": "https://example.com/page"}' \
  "https://minifetch.com/api/v1/x402/extract/url-links"
```

**Response:** JSON object with the source URL and a `links` array containing all discovered URLs on the page.

### Preflight URL Check (Free)

Check whether a URL is fetch-able and respects robots.txt before spending on a paid extraction. This endpoint is free
and does not require x402 payment.

**Pricing:** $0.00

```sh
obulx "https://minifetch.com/api/v1/free/preflight/url-check?url=https://example.com/page"
```

**Response:** JSON object indicating whether the URL is accessible and compliant with robots.txt.

## Endpoint Pricing Reference

| Endpoint                                     | Price  | Purpose                                     |
|----------------------------------------------|--------|---------------------------------------------|
| `POST /api/v1/x402/extract/url-metadata`     | $0.002 | Extract rich metadata from a URL            |
| `POST /api/v1/x402/extract/url-links`        | $0.002 | Discover all links on a page                |
| `GET /api/v1/free/preflight/url-check`        | $0.00  | Check if a URL is fetch-able (free)         |

## When to Use

- **SEO research** — Extract Open Graph tags, Twitter Cards, JSON-LD, hreflang, and robots directives from any URL.
- **Link discovery** — Find all outbound and internal links on a page for crawl planning or competitive analysis.
- **Metadata preview** — Check how a URL will appear when shared on social media (OG tags, Twitter Cards).
- **Lightweight analysis** — When you only need metadata, not full page content. Cheaper than a full scrape.
- **Preflight checks** — Use the free endpoint to verify a URL is accessible before spending on extraction.

## Best Practices

- **Use preflight first** — Call the free `/preflight/url-check` endpoint before paid extraction to avoid paying for
  URLs that are blocked by robots.txt.
- **Default verbosity is sufficient** — Only use `"verbosity": "full"` when you need every available tag. The default
  response covers the most common metadata fields.
- **Combine with Firecrawl** — Use Minifetch for metadata and link discovery, then Firecrawl for full page content
  extraction on the URLs you identify.
- **Not for full content** — Minifetch extracts metadata and links, not full page text. Use Firecrawl, SimpleScraper,
  or x402engine-web for full content scraping.
- **Respects robots.txt** — Minifetch follows ethical scraping practices. URLs blocked by robots.txt will not be
  fetched.

## Error Handling

| Error                       | Cause                                    | Solution                                                                                 |
|-----------------------------|------------------------------------------|------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `400 Bad Request`           | Missing or invalid `url` in request body | Ensure the `url` field is present and is a valid URL.                                    |
| `403 Forbidden`             | URL blocked by robots.txt                | Use the free preflight check first. Choose a different URL that allows scraping.         |
| `422 Unprocessable Entity`  | URL cannot be fetched                    | The target URL may be down or require authentication. Minifetch cannot access login-gated pages. |
| `429 Too Many Requests`     | Rate limit exceeded                      | Add a short delay between requests and avoid rapid-fire calls.                           |
| `500 Internal Server Error` | Upstream Minifetch service issue         | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.   |
