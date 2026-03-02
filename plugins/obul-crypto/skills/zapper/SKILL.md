---
name: obul-zapper
description: "USE THIS SKILL WHEN: the user wants portfolio balances, DeFi app positions, NFT holdings, token prices, transaction history, or account identity data across 60+ chains via Zapper's GraphQL API."
homepage: https://protocol.zapper.xyz
metadata:
  obul-skill:
    emoji: "💰"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# Zapper Portfolio Analytics

Zapper provides enterprise-grade blockchain data across 60+ chains through a single GraphQL API. Access portfolio
balances (tokens, DeFi, NFTs), real-time and historical token prices, transaction history with human-readable
interpretations, NFT valuations, and account identity resolution. Through the Obul proxy, each query is paid
individually with no Zapper account or API key required.

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

Base URL: `https://proxy.obul.ai/proxy/https/public.zapper.xyz`

All Zapper queries use a single GraphQL endpoint at `/graphql`. To get an Obul API key, sign up at
**https://my.obul.ai**.

## Common Operations

### Get Portfolio Token Balances

Retrieve all token balances for a wallet address across multiple chains, with USD valuations and network breakdown.

**Pricing:** $0.003

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/public.zapper.xyz/graphql",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "query": "query Portfolio($addresses: [Address!]!) { portfolioV2(addresses: $addresses) { tokenBalances { totalBalanceUSD byToken(first: 25) { totalCount edges { node { symbol name balanceUSD balance network { name chainId } } } } } } }",
    "variables": {
      "addresses": ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]
    }
  }
}
```

**Response:** JSON object with total portfolio value in USD, broken down by individual token holdings including symbol,
name, balance, USD value, and network details.

### Get DeFi App Balances

Retrieve all DeFi protocol positions for a wallet, including lending, staking, LP positions, and claimable rewards.

**Pricing:** $0.003

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/public.zapper.xyz/graphql",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "query": "query AppBalances($addresses: [Address!]!) { portfolioV2(addresses: $addresses) { appBalances { byApp(first: 25) { totalCount edges { node { appName balanceUSD network { name } } } } } } }",
    "variables": {
      "addresses": ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]
    }
  }
}
```

**Response:** JSON object with DeFi positions grouped by application (Aave, Uniswap, Lido, etc.), including balance
in USD, network, and position type (supplied, borrowed, staked, claimable).

### Get NFT Balances

Retrieve all NFT holdings for a wallet with collection metadata, estimated valuations, and media URLs.

**Pricing:** $0.003

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/public.zapper.xyz/graphql",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "query": "query NftBalances($addresses: [Address!]!) { portfolioV2(addresses: $addresses) { nftBalances { totalTokensOwned byCollection(first: 25) { totalCount edges { node { collection { name floorPrice { valueUsd } } balanceUSD tokenCount } } } } } }",
    "variables": {
      "addresses": ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]
    }
  }
}
```

**Response:** JSON object with total NFTs owned, collections with floor prices, estimated USD value, and token counts.

### Get Token Price and Market Data

Retrieve real-time pricing, market data, holders, and recent swaps for a specific token.

**Pricing:** $0.003

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/public.zapper.xyz/graphql",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "query": "query TokenPrice($address: Address!, $network: Network!) { fungibleToken(address: $address, network: $network) { symbol name price marketCap totalSupply holders { totalCount } } }",
    "variables": {
      "address": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
      "network": "ETHEREUM_MAINNET"
    }
  }
}
```

**Response:** JSON object with token symbol, name, current price, market cap, total supply, and holder count.

### Get Transaction History

Retrieve human-readable transaction history for a wallet with structured asset deltas and interpretations.

**Pricing:** $0.003

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/public.zapper.xyz/graphql",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "query": "query Transactions($addresses: [Address!]!, $first: Int) { transactionHistoryV2(addresses: $addresses, first: $first) { edges { node { hash timestamp network { name } interpretation { description } } } } }",
    "variables": {
      "addresses": ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"],
      "first": 10
    }
  }
}
```

**Response:** JSON object with transaction list including hash, timestamp, network, and human-readable interpretation
of what the transaction did (e.g., "Swapped 1 ETH for 3,200 USDC on Uniswap").

## Endpoint Pricing Reference

| Query                         | Price  | Purpose                                              |
|-------------------------------|--------|------------------------------------------------------|
| `portfolioV2.tokenBalances`   | $0.003 | Token balances across all chains                     |
| `portfolioV2.appBalances`     | $0.003 | DeFi app positions (lending, staking, LP)            |
| `portfolioV2.nftBalances`     | $0.003 | NFT holdings with valuations                         |
| `fungibleToken`               | $0.003 | Token price, market cap, holders                     |
| `transactionHistoryV2`        | $0.003 | Human-readable transaction history                   |
| `accounts`                    | $0.003 | Resolve Farcaster users, ENS, Lens to addresses      |
| `latestSwaps`                 | $0.008 | Recent swap activity for a token                     |
| `latestFarcasterSwaps`        | $0.008 | Recent swaps by Farcaster users                      |
| `generalSwapFeed`             | $0.012 | General swap feed across protocols                   |
| `tokenActivityFeed`           | $0.012 | Token-specific activity feed                         |
| `tokenRanking`                | $0.012 | Trending tokens by volume or activity                |
| `nftRanking`                  | $0.012 | Top NFT collections by volume or floor price         |
| `farcasterHolders`            | $0.012 | Token holders with Farcaster profiles                |

## When to Use

- **Multi-chain portfolio** -- User wants a unified view of token, DeFi, and NFT holdings across 60+ chains.
- **DeFi positions** -- User asks about their lending, staking, LP, or claimable positions across protocols.
- **NFT valuation** -- User wants to see NFT holdings with floor prices and estimated values.
- **Token prices** -- User asks for current price, market cap, or holder data for a specific token.
- **Transaction history** -- User wants human-readable summaries of their on-chain activity.
- **Identity resolution** -- User provides a Farcaster username, ENS name, or Lens handle and needs the associated
  wallet address.
- **Trending tokens and NFTs** -- User asks about what tokens or NFT collections are trending.

## Best Practices

- **Use `portfolioV2` for all portfolio queries** -- The older `portfolio` query is deprecated. Always use
  `portfolioV2`.
- **Query multiple addresses at once** -- Pass an array of addresses to get combined portfolio data in a single request
  instead of making separate calls per wallet.
- **Filter by chain when possible** -- Use the `chainIds` parameter to narrow results to specific networks and reduce
  response size.
- **Paginate large responses** -- Use the `first` parameter to limit results and paginate through large token or NFT
  collections.
- **Use minimal field selection** -- Only request the GraphQL fields you need. Smaller queries are faster and cheaper.
- **Combine portfolio sections** -- Query `tokenBalances`, `appBalances`, and `nftBalances` in a single `portfolioV2`
  call to get the full picture in one request.

## Error Handling

| Error                      | Cause                                    | Solution                                                                                  |
|----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`     | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai.  |
| `400 Bad Request`          | Malformed GraphQL query                  | Validate your GraphQL query syntax. Check field names match the schema.                   |
| `401 Unauthorized`         | Missing or invalid API key               | Ensure the `x-obul-api-key` header is present with a valid key.                           |
| `429 Too Many Requests`    | Rate limit exceeded                      | Reduce request frequency. Portfolio queries are limited to 2-5 RPS depending on tier.     |
| `500 Internal Server Error`| Upstream Zapper service issue            | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.    |
| `503 Service Unavailable`  | Zapper service temporarily down          | Retry after a brief wait.                                                                 |
