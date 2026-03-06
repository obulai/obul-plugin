---
name: obul-blocksec
description: "USE THIS SKILL WHEN: the user wants blockchain address compliance screening, AML risk assessment, address labeling, KYA (Know Your Address) reports, or sanctions checking for Ethereum, Tron, BSC, and other chains."
homepage: https://x402.blocksec.ai
metadata:
  obul-skill:
    emoji: "💰"
registries: {}
---

# BlockSec Address Screening

BlockSec provides blockchain address compliance and risk screening APIs, powered by MetaSleuth and Phalcon Compliance
intelligence. Services include address labeling (identifying exchanges, protocols, and known entities), light risk
screening for quick AML checks, and deep risk screening for comprehensive KYA (Know Your Address) compliance reports.
BlockSec is a reputable Web3 security firm trusted by major protocols. Through the Obul proxy, each request is paid
individually with no BlockSec account or API key required.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://x402.blocksec.ai`

## Common Operations

### Address Labels

Retrieve canonical entity names and categories for a blockchain address. Identifies exchanges, protocols, funds,
and other labeled entities. Useful for understanding who owns or controls an address.

**Pricing:** $0.10

```sh
obulx "https://x402.blocksec.ai/label/eth/0x463452c356322d463b84891ebda33daed274cb40"
```

**Response:** JSON object with entity name (e.g., "Binance"), category (e.g., "Exchange"), subcategories, and
confidence score. Returns empty labels for unlabeled addresses.

### Light Risk Screening

Perform a quick compliance risk assessment for a wallet address. Returns a risk score and basic AML screening results
including interactions with sanctioned or suspicious wallets. Ideal for fast pre-transaction checks.

**Pricing:** $0.20

```sh
obulx "https://x402.blocksec.ai/screen/light/eth/0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
```

**Response:** JSON object with risk score (0-100), risk level (low/medium/high/critical), sanctioned entity
interactions, suspicious activity flags, and a brief risk summary.

### Deep Risk Screening

Deliver a comprehensive KYA (Know Your Address) report with detailed compliance insights, transaction flow analysis,
fund source tracing, and entity relationship mapping. This is the most thorough screening option for due diligence.

**Pricing:** $1.00

```sh
obulx "https://x402.blocksec.ai/screen/deep/tron/TYXqLb9ZyAeJeTFkt3Tx7kNyc3HufjvnMs"
```

**Response:** JSON object with comprehensive KYA report including risk score, detailed risk breakdown, fund source
analysis, transaction flow mapping, sanctioned entity exposure, counterparty analysis, and compliance recommendations.

## Endpoint Pricing Reference

| Endpoint                            | Price | Purpose                                              |
|-------------------------------------|-------|------------------------------------------------------|
| `GET /label/{chain}/{address}`      | $0.10 | Address labeling (entity name, category)             |
| `GET /screen/light/{chain}/{address}` | $0.20 | Quick AML risk screening with risk score           |
| `GET /screen/deep/{chain}/{address}`  | $1.00 | Comprehensive KYA compliance report                |
| `GET /label/eth/{address}`          | $0.10 | Ethereum address labels                              |
| `GET /label/bsc/{address}`          | $0.10 | BSC address labels                                   |
| `GET /label/tron/{address}`         | $0.10 | Tron address labels                                  |
| `GET /health`                       | $0.00 | Server health check                                  |

## When to Use

- **Pre-transaction compliance** -- User wants to check if a counterparty address is safe before sending funds.
- **AML screening** -- User needs to assess whether an address has interacted with sanctioned or suspicious entities.
- **Address identification** -- User wants to know who owns a specific address (exchange, protocol, fund, etc.).
- **KYA due diligence** -- User needs a comprehensive compliance report for regulatory or audit purposes.
- **Sanctions checking** -- User wants to verify an address is not on any sanctions lists.
- **Risk assessment** -- User asks about the risk level of interacting with a specific wallet.
- **Tron compliance** -- User needs address screening on Tron, which is commonly associated with compliance concerns.

## Best Practices

- **Start with labels, then screen** -- Use `/label` first ($0.10) to identify the entity. Only escalate to screening
  if the address is unlabeled or suspicious.
- **Use light screening for routine checks** -- The `/screen/light` endpoint ($0.20) is sufficient for most
  pre-transaction compliance checks.
- **Reserve deep screening for high-value interactions** -- The `/screen/deep` endpoint ($1.00) provides comprehensive
  analysis but is significantly more expensive. Use it for large transactions or regulatory requirements.
- **Check multiple chains** -- Supported chains include eth, bsc, tron, polygon, arbitrum, base, and optimism. Check
  the relevant chain for the address in question.
- **Cache results for known addresses** -- Address labels and risk scores do not change frequently. Cache results for
  addresses you interact with regularly.
- **Combine with other skills** -- Use BlockSec screening before executing swaps via HeyElsa or analyzing wallets
  with SlamAI to ensure compliance.

## Error Handling

| Error                      | Cause                                    | Solution                                                                                  |
|----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`     | Payment not processed or insufficient    | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `400 Bad Request`          | Invalid chain or address format          | Ensure the chain parameter is valid (eth, bsc, tron, etc.) and the address is correctly formatted. |
| `404 Not Found`            | Unsupported chain                        | Verify the chain is in the supported list: eth, bsc, tron, polygon, arbitrum, base, optimism. |
| `422 Unprocessable Entity` | Valid request but cannot process          | The address format may not match the specified chain (e.g., 0x address for Tron).         |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests and avoid rapid-fire calls.                            |
| `500 Internal Server Error`| Upstream BlockSec service issue          | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.    |
