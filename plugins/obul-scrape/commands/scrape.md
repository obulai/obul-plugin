---
name: scrape
description: Scrape a webpage and extract content. Usage: /obul-scrape:scrape <URL>
---

Scrape a webpage and return its content using the best available x402 scraping provider.

## Usage

```
/obul-scrape:scrape https://example.com/page
/obul-scrape:scrape https://example.com/page --format json --prompt "Extract pricing info"
/obul-scrape:scrape https://example.com --crawl --limit 10
/obul-scrape:scrape https://example.com --map
/obul-scrape:scrape https://example.com/page --metadata
```

## Provider Selection

Choose the provider based on what the user needs:

### x402engine-web (`x402engine-web` skill)
**Best for: quick single-page scrape to markdown**
- Default choice for simple "scrape this URL" requests
- Cheapest option at $0.005/request
- Returns clean markdown, no configuration needed

### Firecrawl (`firecrawl` skill)
**Best for: advanced scraping, crawling, extraction**
- User wants multiple output formats (markdown, HTML, links, screenshot)
- User wants to crawl an entire site (`--crawl`)
- User wants to discover all URLs on a domain (`--map`)
- User wants AI-powered structured data extraction (`--format json`)
- User needs to filter content with CSS selectors

### SimpleScraper (`simplescraper` skill)
**Best for: structured JSON output**
- User wants structured text and metadata as JSON
- Alternative when other providers have issues with a URL

### Zyte (`zyte` skill)
**Best for: anti-bot protected sites**
- Target site blocks normal scrapers (Cloudflare, Akamai, etc.)
- User needs JavaScript rendering for SPAs
- User needs geo-targeted scraping from a specific country
- Other scrapers are failing on the target URL

### Minifetch (`minifetch` skill)
**Best for: metadata and link extraction only**
- User only needs page metadata (title, OG tags, JSON-LD) — not full content
- User wants to discover links on a page
- User wants the cheapest option for lightweight extraction ($0.002/request)

## Workflow

1. Ensure you are logged in via `obulx login` (see setup rule)
2. Determine which provider best fits the request using the guide above
3. If ambiguous, default to **x402engine-web** for simple scrapes or **Firecrawl** for anything more complex
4. Execute the request through the Obul proxy
5. Present extracted content clearly — if markdown, render it; if JSON, format it
6. If the scrape fails (403, bot detection), suggest retrying with **Zyte** for anti-bot capability
7. Include the cost of the request if returned in the response
