---
name: obul-exa
description: "USE THIS SKILL WHEN: the user wants neural or semantic web search, finding similar pages, retrieving page contents, or getting AI-generated answers with citations. Provides intelligent search via Exa through the Obul proxy."
homepage: https://exa.ai
metadata:
  obul-skill:
    emoji: "🔍"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# Exa

Exa provides neural and semantic web search using AI-powered search engine. Exa excels at semantic/meaning-based search, finding pages similar to a URL, extracting page contents, and generating answers with citations. No API key needed — payment is handled automatically by the Obul proxy.

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

Base URL: `https://proxy.obul.ai/proxy/https/api.exa.ai`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Search

Perform a neural or semantic web search with optional content extraction.

**Pricing:** $0.005

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/api.exa.ai/search",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "query": "latest research on LLM reasoning",
    "type": "auto",
    "numResults": 5,
    "contents": {
      "text": true
    }
  }
}
```

**Response:** JSON with search results including URL, title, published date, author, text, highlights, and summary.

### Find Similar

Find pages similar to a given URL based on semantic similarity.

**Pricing:** $0.005

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/api.exa.ai/findSimilar",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://example.com/interesting-article",
    "numResults": 5,
    "contents": {
      "text": true
    }
  }
}
```

**Response:** Same format as Search with semantically similar pages.

### Get Contents

Extract clean text content from specific URLs.

**Pricing:** $0.002

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/api.exa.ai/contents",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "urls": ["https://example.com/page1", "https://example.com/page2"],
    "text": true
  }
}
```

**Response:** JSON with extracted page content for each URL.

### Answer

Get an AI-generated answer backed by web search citations.

**Pricing:** $0.01

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/api.exa.ai/answer",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "query": "What are the latest developments in x402 payments?",
    "text": true
  }
}
```

**Response:** JSON object with an AI-generated answer and an array of cited source results.

## Endpoint Pricing Reference

| Endpoint            | Price  | Purpose                                |
|---------------------|--------|----------------------------------------|
| `POST /search`      | $0.005 | Neural/semantic web search             |
| `POST /findSimilar` | $0.005 | Find similar pages by URL              |
| `POST /contents`   | $0.002 | Extract content from URLs              |
| `POST /answer`     | $0.01  | AI answer with citations               |

## When to Use

- **Semantic search** — User needs intelligent search finding pages by meaning, not just keywords
- **Similar pages** — User wants to find pages similar to a given URL
- **Content extraction** — User needs to extract clean text content from specific URLs
- **AI answers** — User wants an AI-generated answer backed by web search citations
- **Research** — User asks for research papers, company pages, news, or tweets by category

## Best Practices

- **Use search types wisely** — Use `"type": "keyword"` for exact-match searches, `"neural"` for semantic, `"auto"` to let Exa decide
- **Use category filter** — The `category` parameter significantly improves result quality when you know the content type
- **Use findSimilar** — This is uniquely powerful for discovering related content from a known good URL
- **Use contents when you have URLs** — The contents endpoint is useful when you already have URLs and just need to extract their text
- **Max characters** — Use `{"maxCharacters": 2000}` in contents.text to limit response size

## Error Handling

| Error                       | Cause                                    | Solution                                                                                  |
|-----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai.   |
| `400 Bad Request`           | Invalid request body                    | Ensure required fields are present and correctly formatted.                                |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests.                                                       |
| `500 Internal Server Error` | Exa service issue                        | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
