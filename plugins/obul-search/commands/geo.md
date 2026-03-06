---
description: Check brand visibility across AI search engines (ChatGPT, Perplexity, Gemini, Claude, Grok). Usage: /obul-search:geo <brand> [prompt]
---

# GEO Visibility Check

Check how a brand appears across AI search engines via the GEO visibility tracking service.

**Cost:** $0.03 per visibility check

## Usage

`/obul-search:geo <brand>` — Auto-generate a relevant prompt and check visibility
`/obul-search:geo <brand> <prompt>` — Check visibility for a specific prompt

## Workflow

1. Parse `$ARGUMENTS` to extract the brand name and optional prompt
   - If only a brand is given, generate a natural search prompt like "What is the best [category] tool?" based on the brand
   - If a prompt is given, use it directly
2. Run the visibility check:

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{
    "prompt": "<the prompt>",
    "brand": "<the brand>",
    "models": ["perplexity", "openai", "gemini", "claude", "grok"]
  }' \
  "https://geo.x402endpoints.com/v1/visibility/check"
```

3. Present results as a summary table:
   - **Mention rate** across models (e.g., "4/5 models mentioned the brand")
   - **Position** — where in the response the brand appears (first paragraph is best)
   - **Sentiment** — how models describe the brand
   - **Share of Voice** — brand vs competitor mentions if competitors were included
   - **Citations** — any URLs linked to the brand
4. If the brand was not mentioned by most models, suggest running `/obul-search:geo-suggest` to get prompt optimization ideas
