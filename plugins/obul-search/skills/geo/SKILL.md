---
name: geo
description: GEO (Generative Engine Optimization) visibility tracking. Use when the user wants to check how a brand appears across AI search engines (ChatGPT, Perplexity, Gemini, Claude, Grok), analyze sentiment, track competitors, get prompt suggestions, or analyze citations.
homepage: https://geo.x402endpoints.com
metadata:
  obul-skill:
    emoji: "📊"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# GEO Visibility Tracking

Track how your brand appears across AI search engines — ChatGPT, Perplexity, Gemini, Claude, and Grok. GEO fans out a prompt to multiple LLMs simultaneously and analyzes mentions, position, sentiment, citations, competitor share of voice, and more. Pay-per-use through the Obul proxy.

## When to Use
- User wants to check if/how a brand is mentioned by AI search engines
- User wants to compare brand visibility against competitors across LLMs
- User needs sentiment analysis of how AI models talk about a brand
- User wants prompt suggestions that would lead to brand mentions
- User wants to analyze citation URLs that AI models reference
- User is doing GEO/AIO (AI optimization) research or auditing

## Endpoints

### Visibility Check
- **URL**: `https://geo.x402endpoints.com/v1/visibility/check`
- **Method**: POST
- **Pricing**: $0.03 per request

Fan out a prompt to multiple LLMs and get a full visibility report: mentions, paragraph position, sentiment, citations with linked URLs, competitor analysis, and share of voice.

**Request:**
```bash
curl -sS -X POST \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: ${OBUL_API_KEY}" \
  -d '{
    "prompt": "What is the best project management tool for startups?",
    "brand": "Linear",
    "aliases": ["linear.app"],
    "competitors": ["Jira", "Asana", "Monday"],
    "models": ["perplexity", "openai", "gemini", "claude", "grok"]
  }' \
  "https://proxy.obul.ai/proxy/https/geo.x402endpoints.com/v1/visibility/check"
```

**Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | The search prompt to test |
| `brand` | string | required | Brand name to track |
| `aliases` | array | `[]` | Alternative brand names or domains |
| `competitors` | array | `[]` | Competitor names to track |
| `models` | array | all 5 | Which LLMs to query: `"perplexity"`, `"openai"`, `"gemini"`, `"claude"`, `"grok"` |

**Response:**
```json
{
  "brand": "Linear",
  "prompt": "What is the best project management tool for startups?",
  "timestamp": "2026-03-02T12:00:00.000Z",
  "models": [
    {
      "modelId": "perplexity",
      "modelName": "Perplexity Sonar",
      "mention": {
        "mentioned": true,
        "position": 0.0,
        "paragraphPosition": 1,
        "positionLabel": "first_paragraph",
        "count": 3,
        "matchedTerm": "Linear"
      },
      "sentiment": {
        "label": "positive",
        "confidence": 0.6
      },
      "citations": {
        "markers": ["[1]", "[2]"],
        "urls": ["https://linear.app", "https://example.com/review"],
        "linked": [
          { "marker": "[1]", "url": "https://linear.app" },
          { "marker": "[2]", "url": "https://example.com/review" }
        ]
      },
      "competitors": [
        { "name": "Jira", "mentioned": true, "count": 2 },
        { "name": "Asana", "mentioned": true, "count": 1 },
        { "name": "Monday", "mentioned": false, "count": 0 }
      ],
      "latencyMs": 1200,
      "rawTextLength": 850
    }
  ],
  "errors": [],
  "summary": {
    "mentionRate": 0.8,
    "avgPosition": 0.1,
    "dominantSentiment": "positive",
    "totalModelsQueried": 5,
    "totalModelsSucceeded": 5,
    "shareOfVoice": {
      "brand": 0.5,
      "competitors": [
        { "name": "Jira", "share": 0.3 },
        { "name": "Asana", "share": 0.15 },
        { "name": "Monday", "share": 0.05 }
      ]
    }
  }
}
```

### Prompt Suggest
- **URL**: `https://geo.x402endpoints.com/v1/prompts/suggest`
- **Method**: POST
- **Pricing**: $0.01 per request

Generate AI-powered prompt suggestions that would lead to brand mentions. Useful for content strategy and GEO optimization.

**Request:**
```bash
curl -sS -X POST \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: ${OBUL_API_KEY}" \
  -d '{
    "url": "https://linear.app",
    "brand": "Linear",
    "stage": "consideration"
  }' \
  "https://proxy.obul.ai/proxy/https/geo.x402endpoints.com/v1/prompts/suggest"
```

**Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `url` | string | required | Brand website URL |
| `brand` | string | required | Brand name |
| `stage` | string | `"awareness"` | Funnel stage: `"awareness"`, `"consideration"`, `"decision"` |

**Response:**
```json
{
  "brand": "Linear",
  "url": "https://linear.app",
  "prompts": [
    {
      "prompt": "What are the fastest issue trackers for engineering teams?",
      "stage": "consideration",
      "rationale": "Targets Linear's core differentiator of speed and developer experience"
    }
  ]
}
```

### Citation Analyze
- **URL**: `https://geo.x402endpoints.com/v1/citations/analyze`
- **Method**: POST
- **Pricing**: $0.01 per request

Fetch and analyze citation URLs for content quality metrics — word count, entity density, structured data, and freshness.

**Request:**
```bash
curl -sS -X POST \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: ${OBUL_API_KEY}" \
  -d '{
    "urls": [
      "https://linear.app/blog/post-1",
      "https://example.com/linear-review"
    ]
  }' \
  "https://proxy.obul.ai/proxy/https/geo.x402endpoints.com/v1/citations/analyze"
```

**Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `urls` | array | required | Citation URLs to analyze (1-10) |

**Response:**
```json
{
  "citations": [
    {
      "url": "https://linear.app/blog/post-1",
      "title": "Why Linear is Fast",
      "wordCount": 1200,
      "entityDensity": 0.15,
      "hasStructuredData": true,
      "freshness": "2026-02-15"
    }
  ]
}
```

## Endpoint Pricing Reference

| Endpoint | Price | Purpose |
|---|---|---|
| `/v1/visibility/check` | $0.03 | Full visibility analysis across up to 5 LLMs |
| `/v1/prompts/suggest` | $0.01 | AI-generated prompt suggestions for brand visibility |
| `/v1/citations/analyze` | $0.01 | Analyze citation URL quality metrics |
| `/health` | $0.00 | Health check |
| `/pricing` | $0.00 | Get per-endpoint pricing |
| `/openapi.json` | $0.00 | OpenAPI 3.1 specification |

## Best Practices
- Start with a visibility check using all 5 models to get a baseline, then narrow to specific models for follow-up
- Use `aliases` for brands with common abbreviations (e.g., "Amazon Web Services" + "AWS")
- Include 2-4 direct competitors to get meaningful share of voice data
- Use the `positionLabel` field to quickly assess brand prominence ("first_paragraph" is ideal)
- Chain visibility check → prompt suggest → re-check to measure optimization impact
- The `linked` array in citations tells you which URLs are actually cited by number — more actionable than raw URL lists

## Error Handling

| Error | Cause | Solution |
|---|---|---|
| `400` | Missing required fields (`prompt`, `brand`, `url`, `urls`) | Check request body |
| `402` | No payment header or insufficient USDC balance | Ensure `x-obul-api-key` is set and funded |
| `429` | Rate limit exceeded | Wait and retry after `Retry-After` header value |
| `502` | Upstream LLM failed | Retry; check `errors` array for per-model failures |
| `503` | Service misconfigured | Contact service operator |
