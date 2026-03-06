---
name: exa
description: Neural and semantic web search using Exa via StableEnrich (enrichx402). Use when the user needs intelligent search, retrieving page contents, or getting AI-generated answers with citations.
---

# exa Skill

Search the web using Exa's neural search engine through StableEnrich's x402-enabled API. Exa excels at semantic/meaning-based search, extracting page contents, and generating answers with citations.

## When to Use
- User needs semantic or neural search (finding pages by meaning, not just keywords)
- User needs to extract clean text content from specific URLs
- User wants an AI-generated answer backed by web search citations
- User asks for research papers, company pages, news, or tweets by category

## Endpoints

### Search
- **URL**: `https://stableenrich.dev/api/exa/search`
- **Method**: POST
- **Pricing**: ~$0.005 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{
    "query": "latest research on LLM reasoning",
    "type": "auto",
    "numResults": 5,
    "contents": {
      "text": true
    }
  }' \
  "https://stableenrich.dev/api/exa/search"
```

**Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | required | Search query |
| `type` | string | `"auto"` | `"neural"`, `"auto"`, `"keyword"` |
| `numResults` | integer | 10 | Number of results (max 100) |
| `category` | string | — | `"company"`, `"research paper"`, `"news"`, `"tweet"`, `"personal site"`, `"financial report"`, `"people"` |
| `includeDomains` | array | — | Restrict results to these domains |
| `excludeDomains` | array | — | Exclude these domains |
| `startPublishedDate` | string | — | ISO 8601 date filter start |
| `endPublishedDate` | string | — | ISO 8601 date filter end |
| `contents.text` | bool/object | — | Include full text. Object form: `{"maxCharacters": 2000}` |
| `contents.highlights` | bool/object | — | Include relevant snippets. Object form: `{"maxCharacters": 500, "query": "specific focus"}` |
| `contents.summary` | object | — | LLM-generated summary. `{"query": "summarize the key findings"}` |

**Response:**
```json
{
  "requestId": "abc123",
  "resolvedSearchType": "neural",
  "results": [
    {
      "id": "https://example.com/article",
      "url": "https://example.com/article",
      "title": "Article Title",
      "author": "Author Name",
      "text": "Full page content...",
      "highlights": ["Relevant snippet 1", "Relevant snippet 2"],
      "summary": "LLM-generated summary of the page"
    }
  ],
  "costDollars": { "total": 0.005 }
}
```

### Get Contents
- **URL**: `https://stableenrich.dev/api/exa/contents`
- **Method**: POST
- **Pricing**: ~$0.001 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{
    "urls": ["https://example.com/page1", "https://example.com/page2"],
    "text": true
  }' \
  "https://stableenrich.dev/api/exa/contents"
```

**Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `urls` | array | required | URLs to extract content from |
| `text` | bool/object | — | Include full text. Object form: `{"maxCharacters": 5000}` |
| `highlights` | bool/object | — | Include relevant snippets |
| `summary` | object | — | LLM-generated summary |
| `maxAgeHours` | integer | — | Cache freshness; triggers live crawl if stale |
| `subpages` | integer | — | Number of subpages to also crawl |

**Response:**
```json
{
  "results": [
    {
      "url": "https://example.com/page1",
      "title": "Page Title",
      "author": "Author Name",
      "text": "Full extracted page content..."
    }
  ],
  "costDollars": { "total": 0.001 }
}
```

### Answer
- **URL**: `https://stableenrich.dev/api/exa/answer`
- **Method**: POST
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{
    "query": "What are the latest developments in x402 payments?",
    "text": true
  }' \
  "https://stableenrich.dev/api/exa/answer"
```

**Response:** JSON object with an AI-generated answer and an array of cited source results.

## Notes
- Exa's neural search is best for semantic queries where meaning matters more than exact keyword matching
- Use `"type": "keyword"` for exact-match searches, `"neural"` for semantic, `"auto"` to let Exa decide
- The `category` parameter significantly improves result quality when you know the content type
- `contents` is useful when you already have URLs and just need to extract their text
- All endpoints route through StableEnrich's x402-enabled API at `stableenrich.dev`
- Settlement is in USDC on Base chain, handled automatically by the Obul proxy
