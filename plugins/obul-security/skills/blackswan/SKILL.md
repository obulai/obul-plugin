---
name: blackswan
description: Get real-time risk intelligence and market safety signals from BlackSwan. Use when users need crypto market risk assessment, precursor detection, or state synthesis before taking action.
---

# blackswan Skill

Real-time risk intelligence infrastructure for autonomous AI agents. BlackSwan monitors prediction markets, derivatives data, social signals, and news to assess whether it is safe to act in crypto markets.

## When to Use
- User asks whether market conditions are safe before executing a trade or action
- User needs a quick risk signal or precursor detection (short-term, ~15 min window)
- User needs deeper state synthesis and risk assessment (longer horizon, ~1 hour)
- User mentions "risk assessment", "market safety", "black swan event", or "threat intelligence"

## Endpoints

### Flare -- Precursor Detection
- **URL**: `https://x402.blackswan.wtf/smart-agents/flare`
- **Method**: POST
- **Pricing**: ~$0.01 USDC per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{}' \
  "https://x402.blackswan.wtf/smart-agents/flare"
```

**Response:** JSON object with precursor detection signals covering a ~15-minute window. Indicates early warning signs of market disruption or anomalous activity.

### Core -- State Synthesis
- **URL**: `https://x402.blackswan.wtf/smart-agents/core`
- **Method**: POST
- **Pricing**: ~$0.03 USDC per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{}' \
  "https://x402.blackswan.wtf/smart-agents/core"
```

**Response:** JSON object with a comprehensive state synthesis covering up to a ~1-hour horizon. Combines prediction market data, derivatives, social signals, and news into an overall risk assessment.

### Agent Info (Free)
- **URL**: `https://x402.blackswan.wtf/smart-agents/{agent}`
- **Method**: GET
- **Pricing**: Free

**Request:**
```sh
obulx "https://x402.blackswan.wtf/smart-agents/flare"
```

**Response:** Agent metadata including description, capabilities, and pricing.

## Notes
- All requests go through the Obul proxy; do not call `x402.blackswan.wtf` directly.
- Flare is faster and cheaper for quick checks; Core provides deeper analysis at higher cost.
- Payment is in USDC on Base (EIP-155:8453), handled automatically by the proxy.
- BlackSwan also supports MCP integration at `mcp.blackswan.wtf/mcp` and ACP via Virtuals, but through Obul use the x402 endpoints above.
