---
description: Obul CLI setup and verification
---

# Obul CLI Setup

## Install the CLI

All Obul skills use the `obulx` CLI, which handles proxy routing and authentication automatically.

```sh
npm install -g @obul.ai/obulx
```

## Log In

Authenticate via OAuth device flow:

```sh
obulx login
```

Follow the on-screen instructions: visit the URL shown, enter the code, and authorize. Once complete, `obulx` stores your credentials locally — no environment variables needed.

## Verification

Confirm your identity:

```sh
obulx whoami
```

Test with a real request:

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"url": "https://example.com", "formats": ["markdown"]}' \
  "https://firecrawl.x402endpoints.com/v1/scrape" | head -c 200
```

A successful response returns JSON with scraped content.

## Troubleshooting

| Problem | Solution |
|---|---|
| `401 Unauthorized` | Run `obulx login` to re-authenticate |
| `402 Payment Required` | Your Obul balance may be depleted — top up at [my.obul.ai](https://my.obul.ai) |
| `403 Forbidden` | Your account may not have access to this service |
| `obulx: command not found` | Run `npm install -g @obul.ai/obulx` to install the CLI |

## Spending Controls

Obul supports spending caps. Manage your limits in the [dashboard](https://my.obul.ai) to control per-day spending.
