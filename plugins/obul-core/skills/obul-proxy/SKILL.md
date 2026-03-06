---
name: obul-proxy
description: "USE THIS SKILL WHEN: the user wants to proxy a request through Obul, call an x402 API directly, or needs to understand the Obul proxy URL pattern. Handles x402 payment negotiation automatically."
homepage: https://obul.ai
metadata:
  obul-skill:
    emoji: "🔗"
registries: {}
---

# Obul Proxy

Proxy any upstream request through Obul; Obul handles x402 discovery and payment flow automatically.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://<upstream-host>`

## Common Operations

### Health Check

Verify the Obul proxy is operational.

**Pricing:** $0.00

```sh
obulx "https://proxy.obul.ai/healthz"
```

**Response:** Returns `{"status":"ok"}` when the proxy is healthy.

### Proxy a Request

Forward any HTTP request through the Obul proxy. The proxy handles x402 payment negotiation automatically.

**Pricing:** Varies based on upstream endpoint

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{}' \
  "https://x402.browserbase.com/browser/session/create"
```

**Response:** The proxied response from the upstream x402 endpoint.

## Endpoint Pricing Reference

| Endpoint           | Price    | Purpose                                   |
|--------------------|----------|-------------------------------------------|
| `GET /healthz`     | $0.00    | Health check                              |
| `/*`              | Varies   | Proxy any upstream x402 request           |

## When to Use

- **Calling x402 endpoints** — Route any x402-enabled API call through Obul without handling payments manually.
- **Unified API access** — Use a single authenticated session to access multiple x402-enabled services.
- **Automatic payment handling** — Let Obul negotiate and process payments for per-request micropayments.

## Best Practices

- **Check health before use** — Verify the proxy is operational with `/healthz` if you encounter issues.

## Error Handling

| Error                       | Cause                                 | Solution                                                                                      |
|-----------------------------|---------------------------------------|-----------------------------------------------------------------------------------------------|
| `401 Unauthorized`          | Missing or invalid authentication     | Run `obulx login` to authenticate.                                                            |
| `402 Payment Required`      | Upstream requires payment             | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `403 Forbidden`             | Account lacks permissions             | Check your account has the required scopes.                                                   |
| `404 Not Found`             | Invalid upstream URL                  | Verify the upstream endpoint URL is correct.                                                  |
| `429 Too Many Requests`     | Rate limit exceeded                   | Add a short delay between requests.                                                           |
| `500 Internal Server Error` | Obul proxy issue                      | Retry the request. If persistent, check status at https://proxy.obul.ai/healthz.              |
| `503 Service Unavailable`   | Proxy temporarily down                | Wait a few seconds and retry.                                                                 |
