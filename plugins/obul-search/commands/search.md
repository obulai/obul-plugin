---
description: Search the web and return scraped results via Firecrawl ($0.002/request)
---

# Web Search

Search the web for the given query using Firecrawl. Returns search results with scraped page content.

**Cost:** $0.002 per request

Run this command to search for `$ARGUMENTS`:

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"query": "'"$ARGUMENTS"'"}' \
  "https://firecrawl.x402endpoints.com/v1/search"
```

Parse the JSON response and present the search results to the user. Each result includes a URL, title, and scraped markdown content. Summarize the key findings.
