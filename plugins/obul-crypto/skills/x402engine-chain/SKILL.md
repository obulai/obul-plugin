---
name: obul-x402engine-chain
description: "USE THIS SKILL WHEN: the user wants wallet balances, transactions, PnL, ENS resolution, token prices, or transaction simulation. Provides pay-per-use blockchain operations via x402engine through the Obul proxy."
homepage: https://x402engine.app
metadata:
  obul-skill:
    emoji: "⛓️"
    requires:
      env: []
      primaryEnv: ""
registries: {}
---

# x402engine Chain

x402engine provides pay-per-call blockchain operations. Covers wallet analytics, ENS resolution, token metadata, and transaction simulation. No API key needed — payment is handled automatically via `obulx`.

## Authentication

All requests use the `obulx` CLI, which handles x402 payment automatically.

## Common Operations

### Wallet Balances

Get token balances for a wallet address across supported chains.

**Pricing:** $0.005

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "chain": "ethereum"}' \
  "https://x402-gateway-production.up.railway.app/api/wallet/balances"
```

**Response:** JSON with token balances, USD values, and total portfolio value.

### Wallet Transactions

Get recent transaction history for a wallet address.

**Pricing:** $0.005

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "chain": "ethereum"}' \
  "https://x402-gateway-production.up.railway.app/api/wallet/transactions"
```

**Response:** JSON array of recent transactions with hash, from, to, value, and timestamp.

### Wallet PnL

Get profit and loss analysis for a wallet.

**Pricing:** $0.01

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"}' \
  "https://x402-gateway-production.up.railway.app/api/wallet/pnl"
```

**Response:** JSON with realized/unrealized PnL breakdown per token.

### ENS Resolve

Resolve an ENS name to an Ethereum address.

**Pricing:** $0.001

**Request:**
```sh
obulx "https://x402-gateway-production.up.railway.app/api/ens/resolve?name=vitalik.eth"
```

**Response:** JSON with the resolved Ethereum address.

### ENS Reverse

Reverse resolve an Ethereum address to its ENS name.

**Pricing:** $0.001

**Request:**
```sh
obulx "https://x402-gateway-production.up.railway.app/api/ens/reverse?address=0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
```

**Response:** JSON with the ENS name associated with the address.

### Token Prices (Batch)

Get current prices for multiple tokens.

**Pricing:** $0.005

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"tokens": ["bitcoin", "ethereum", "solana"]}' \
  "https://x402-gateway-production.up.railway.app/api/token/prices"
```

**Response:** JSON with current prices for each requested token. Supports up to 200 tokens per request.

### Token Metadata
- **URL**: `https://x402-gateway-production.up.railway.app/api/token/metadata`
- **Method**: GET
- **Pricing**: ~$0.002 per request

**Request:**
```sh
obulx "https://x402-gateway-production.up.railway.app/api/token/metadata?id=ethereum"
```

**Response:** JSON with token name, symbol, description, links, and contract addresses.

### Crypto Price
- **URL**: `https://x402-gateway-production.up.railway.app/api/crypto/price`
- **Method**: GET
- **Pricing**: ~$0.001 per request

**Request:**
```sh
obulx "https://x402-gateway-production.up.railway.app/api/crypto/price?ids=bitcoin,ethereum"
```

**Response:** JSON with current USD prices for the requested coins.

### Crypto Trending
- **URL**: `https://x402-gateway-production.up.railway.app/api/crypto/trending`
- **Method**: GET
- **Pricing**: ~$0.001 per request

**Request:**
```sh
obulx "https://x402-gateway-production.up.railway.app/api/crypto/trending"
```

**Response:** JSON array of currently trending cryptocurrencies.

### Transaction Simulate

Simulate a transaction before sending to preview the outcome.

**Pricing:** $0.01

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"from": "0x...", "to": "0x...", "value": "1000000000000000000", "data": "0x...", "chain": "ethereum"}' \
  "https://x402-gateway-production.up.railway.app/api/tx/simulate"
```

**Response:** JSON with simulation results including gas estimate, state changes, and potential errors.

## Endpoint Pricing Reference

| Endpoint                         | Price  | Purpose                                  |
|----------------------------------|--------|------------------------------------------|
| `POST /api/wallet/balances`      | $0.005 | Get token balances for a wallet          |
| `POST /api/wallet/transactions`  | $0.005 | Get transaction history                 |
| `POST /api/wallet/pnl`           | $0.01  | Get profit and loss analysis            |
| `GET /api/ens/resolve`           | $0.001 | Resolve ENS name to address              |
| `GET /api/ens/reverse`           | $0.001 | Reverse resolve address to ENS name     |
| `POST /api/token/prices`         | $0.005 | Get batch token prices                  |
| `GET /api/token/metadata`        | $0.002 | Get token metadata                      |
| `GET /api/crypto/price`          | $0.001 | Get crypto prices                      |
| `GET /api/crypto/trending`       | $0.001 | Get trending cryptocurrencies          |
| `POST /api/tx/simulate`          | $0.01  | Simulate a transaction                  |

## When to Use

- **Wallet balances** — User asks for a wallet's token balances or holdings
- **Transaction history** — User asks for a wallet's transaction history
- **PnL analysis** — User wants to check wallet PnL (profit and loss)
- **ENS resolution** — User wants to resolve an ENS name to an address (or reverse)
- **Token prices** — User asks for token prices or metadata
- **Transaction simulation** — User wants to simulate a transaction before sending
- **Crypto prices** — User asks about crypto prices, trending coins, or market search

## Best Practices

- **Supported chains** — Wallet endpoints accept EVM addresses and support multiple chains (ethereum, base, polygon, arbitrum, etc.)
- **ENS is Ethereum-specific** — ENS endpoints work only with Ethereum
- **Token limit** — The token prices endpoint supports up to 200 tokens per request

## Notes
- Prices range from $0.001 to $0.01 per request depending on endpoint complexity.
- Wallet endpoints accept EVM addresses and support multiple chains (ethereum, base, polygon, arbitrum, etc.).
- ENS endpoints are Ethereum-specific.
- The `obulx` CLI handles x402 payment automatically -- no USDC wallet or payment header needed.

## Error Handling

| Error                       | Cause                                    | Solution                                                                                  |
|-----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your obulx setup is correct and your account has sufficient balance at my.obul.ai.  |
| `400 Bad Request`           | Invalid request body                    | Ensure required fields are present and correctly formatted.                                |
| `404 Not Found`             | Address or token not found               | Verify the wallet address or token symbol is correct.                                       |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests.                                                       |
| `500 Internal Server Error` | x402engine service issue                 | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
