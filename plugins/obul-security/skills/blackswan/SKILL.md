---
name: obul-blackswan
description: "USE THIS SKILL WHEN: the user wants real-time risk intelligence, market safety signals, precursor detection, or state synthesis. Provides pay-per-use risk intelligence via BlackSwan through the Obul proxy."
homepage: https://x402.blackswan.wtf
metadata:
  obul-skill:
    emoji: "🦢"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# BlackSwan

BlackSwan provides real-time risk intelligence for autonomous AI agents. Monitor prediction markets, derivatives data, social signals, and news to assess whether it is safe to act in crypto markets. No API key needed — payment is handled automatically by the Obul proxy.

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

Base URL: `https://proxy.obul.ai/proxy/https/x402.blackswan.wtf`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Flare -- Precursor Detection

Get quick risk signals with precursor detection for a short-term window (~15 minutes).

**Pricing:** $0.01

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/x402.blackswan.wtf/smart-agents/flare",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {}
}
```

**Response:** JSON object with precursor detection signals covering a ~15-minute window.

### Core -- State Synthesis

Get comprehensive state synthesis for a longer horizon (~1 hour).

**Pricing:** $0.03

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/x402.blackswan.wtf/smart-agents/core",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {}
}
```

**Response:** JSON object with comprehensive state synthesis covering up to ~1 hour.

### Agent Info (Free)

Get agent metadata including description, capabilities, and pricing.

**Pricing:** $0.00

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/x402.blackswan.wtf/smart-agents/flare",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** Agent metadata including description, capabilities, and pricing.

## Endpoint Pricing Reference

| Endpoint                    | Price | Purpose                              |
|-----------------------------|-------|--------------------------------------|
| `POST /smart-agents/flare` | $0.01 | Precursor detection (~15 min window) |
| `POST /smart-agents/core`   | $0.03 | State synthesis (~1 hour horizon)   |
| `GET /smart-agents/{agent}` | $0.00 | Agent metadata (free)               |

## When to Use

- **Risk assessment** — User asks whether market conditions are safe before executing a trade or action
- **Quick signals** — User needs a quick risk signal or precursor detection (short-term)
- **Deep analysis** — User needs deeper state synthesis and risk assessment (longer horizon)
- **Threat intelligence** — User mentions "risk assessment", "market safety", "black swan event", or "threat intelligence"

## Best Practices

- **Use Flare for quick checks** — Flare is faster and cheaper for quick checks
- **Use Core for deep analysis** — Core provides deeper analysis at higher cost
- **Pre-trade checks** — Always check risk signals before executing significant trades

## Error Handling

| Error                       | Cause                                    | Solution                                                                                  |
|-----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai.   |
| `400 Bad Request`           | Invalid request body                    | Ensure the request body is properly formatted.                                            |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests.                                                       |
| `500 Internal Server Error` | BlackSwan service issue                 | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
