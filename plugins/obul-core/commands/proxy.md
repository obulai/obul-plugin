---
description: Proxy any HTTP request through Obul to an x402 API endpoint (cost varies by upstream)
---

# Obul Proxy

Proxy an HTTP request through Obul to any x402-enabled API. Obul handles payment negotiation automatically.

**Cost:** Varies by upstream endpoint

The user provides `$ARGUMENTS` which should be a full upstream URL (e.g., `https://some-x402-service.com/api/endpoint`).

Run the request using obulx:

```sh
obulx "$ARGUMENTS"
```

If the user provides a request body or specific HTTP method, adjust the command accordingly (add `-X POST`, `-d '{...}'`, etc.).

Parse the JSON response and present the result to the user. If the response contains an error, explain what went wrong.
