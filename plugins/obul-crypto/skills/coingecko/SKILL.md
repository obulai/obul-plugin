---
name: obul-coingecko
description: "USE THIS SKILL WHEN: the user wants cryptocurrency prices, on-chain token data, trending pools, or pool search. Provides pay-per-use crypto market data via CoinGecko through the Obul proxy."
homepage: https://www.coingecko.com
metadata:
  obul-skill:
    emoji: "₿"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# CoinGecko

CoinGecko provides pay-per-request cryptocurrency market data including prices, on-chain token data, trending pools, and pool search. No API key needed — payment is handled automatically by the Obul proxy.

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

Base URL: `https://proxy.obul.ai/proxy/https/pro-api.coingecko.com`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Simple Price

Fetch current prices for one or more cryptocurrencies.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/simple/price",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "query": {
    "ids": "bitcoin,ethereum",
    "vs_currencies": "usd",
    "include_24hr_change": "true"
  }
}
```

**Response:** JSON object keyed by coin ID with price, market cap, volume, and change fields.

### On-Chain Token Price

Retrieve token price by contract address on a specific network.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/onchain/simple/networks/base/token_price/0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON with token price, market cap, volume, and liquidity data.

### Token Data by Address

Get detailed token data including price, liquidity, and market metrics.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/onchain/networks/eth/tokens/0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON with detailed token data including price, liquidity, and market metrics.

### Search Pools

Search for DeFi pools by contract address, token name, or symbol.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/onchain/search/pools",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "query": {
    "query": "USDC"
  }
}
```

**Response:** JSON array of matching pools with token pair info, volume, and liquidity.

### Trending Pools by Network

Get trending pools on a specific blockchain network.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/onchain/networks/solana/trending_pools",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON array of trending pools on the specified network with volume and price data.

## Endpoint Pricing Reference

| Endpoint                                                      | Price | Purpose                                   |
|--------------------------------------------------------------|-------|-------------------------------------------|
| `GET /api/v3/x402/simple/price`                              | $0.01 | Get cryptocurrency prices                 |
| `GET /api/v3/x402/onchain/simple/networks/{network}/token_price/{address}` | $0.01 | Token price by contract address |
| `GET /api/v3/x402/onchain/networks/{network}/tokens/{address}` | $0.01 | Detailed token data                      |
| `GET /api/v3/x402/onchain/search/pools`                      | $0.01 | Search DeFi pools                        |
| `GET /api/v3/x402/onchain/networks/{network}/trending_pools` | $0.01 | Trending pools by network                |

## When to Use

- **Price lookup** — User asks for the current price of a cryptocurrency (e.g., "What's the price of Bitcoin?")
- **On-chain data** — User wants token price or liquidity data by contract address
- **Pool search** — User wants to find or search for DeFi pools or tokens
- **Trending pools** — User asks about trending pools on a specific network
- **Market data** — User needs market cap, volume, or 24h change data

## Best Practices

- **Use coin IDs** — CoinGecko coin IDs are lowercase slugs, e.g., `bitcoin`, `ethereum`, `solana`, `usd-coin`
- **Specify networks** — Network IDs for on-chain endpoints: `eth`, `base`, `solana`, `polygon_pos`, `arbitrum`, `optimism`, `bsc`, `avalanche`, and 250+ others
- **Include additional data** — Use `include_market_cap`, `include_24hr_vol`, and `include_24hr_change` to get more context

## Error Handling

| Error                       | Cause                                    | Solution                                                                                  |
|-----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai.   |
| `400 Bad Request`           | Invalid coin ID or network               | Ensure coin IDs are valid slugs and networks are supported.                                |
| `404 Not Found`             | Token or pool not found                  | Verify the contract address or search query is correct.                                    |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests.                                                       |
| `500 Internal Server Error` | CoinGecko service issue                  | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
