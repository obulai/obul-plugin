---
name: obul-slamai
description: "USE THIS SKILL WHEN: the user wants smart money intelligence, wallet reputation scoring, WalletDNA behavioral analysis, token trades and transfers with smart money enrichment, trending or newest tokens, or token holder reputation data."
homepage: https://www.slamai.xyz
metadata:
  obul-skill:
    emoji: "💰"
registries: {}
---

# SlamAI Smart Money Intelligence

SlamAI is the definitive platform for smart money intelligence, live on Base and Ethereum. It provides real-time
on-chain analytics enriched with WalletDNA behavioral metrics, including wallet reputation scoring, smart money trade
tracking, token transfer analysis, trending and newest token discovery, and token holder reputation data. Through the
Obul proxy, each request is paid individually with no SlamAI account required.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://api.slamai.dev`

## Common Operations

### Get Token Price

Retrieve current token prices with fully diluted valuation, liquidity, and 24h price change. Supports quoting in USD,
ETH, or BTC.

**Pricing:** $0.0001

```sh
obulx "https://api.slamai.dev/token/price?blockchain=ethereum&symbols=ETH,USDC&quote_symbol=USD"
```

**Response:** JSON object with current price, FDV, liquidity, and 24h price change for each requested token.

### Get Smart Money Trades (DNA-Enriched)

Retrieve recent trades for a token enriched with WalletDNA metrics, revealing whether buyers or sellers are smart
money wallets based on IQ score, reputation tier, and historical performance.

**Pricing:** $0.003

```sh
obulx "https://api.slamai.dev/token/trades/dna?blockchain=base&token_address=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&num=20"
```

**Response:** JSON array of trades with wallet address, side (buy/sell), amount, price, and WalletDNA enrichment
including IQ score, reputation tier, win rate, and wallet balance.

### Get Wallet Reputation

Retrieve the full reputation profile for a wallet address, including IQ score, reputation tier, trading history,
win rate, and behavioral classification.

**Pricing:** $0.001

```sh
obulx "https://api.slamai.dev/wallet/reputation/full?blockchain=ethereum&wallet_address=0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
```

**Response:** JSON object with wallet IQ score, reputation tier (e.g., "Smart Money", "Veteran", "Newcomer"), total
trades, win rate, average hold time, and behavioral DNA classification.

### Get Trending Tokens

Retrieve currently trending tokens on a specific blockchain, ranked by smart money activity and volume.

**Pricing:** $0.0005

```sh
obulx "https://api.slamai.dev/chain/tokens/trending?blockchain=base&num=20"
```

**Response:** JSON array of trending tokens with symbol, name, address, price, volume, and smart money activity score.

### Get Token Transfers with DNA

Retrieve token transfers enriched with WalletDNA behavioral data, showing whether large transfers are from smart
money wallets.

**Pricing:** $0.001

```sh
obulx "https://api.slamai.dev/token/transfers/dna?blockchain=ethereum&token_address=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&num=20"
```

**Response:** JSON array of transfers with sender, receiver, amount, direction, and WalletDNA enrichment for each
wallet involved.

## Endpoint Pricing Reference

| Endpoint                            | Price   | Purpose                                          |
|-------------------------------------|---------|--------------------------------------------------|
| `GET /token/price`                  | $0.0001 | Current token prices with FDV and liquidity      |
| `GET /token/price/history`          | $0.0005 | Historical price at specific block or timestamp  |
| `GET /token/price/exotic`           | $0.0005 | Price quoted in any ERC20 token                  |
| `GET /token/price/exotic/history`   | $0.0005 | Historical exotic pricing                        |
| `GET /token/trades`                 | $0.0005 | Recent trades for a token                        |
| `GET /token/trades/dna`             | $0.003  | Trades with WalletDNA smart money enrichment     |
| `GET /token/transfers`              | $0.0003 | Token transfer records                           |
| `GET /token/transfers/dna`          | $0.001  | Transfers with WalletDNA enrichment              |
| `GET /token/holder/reputation`      | $0.001  | Reputation scores for token holders              |
| `GET /chain/tokens/trending`        | $0.0005 | Trending tokens by smart money activity          |
| `GET /chain/tokens/popular`         | $0.0005 | Most popular tokens by volume                    |
| `GET /chain/tokens/popular/history` | $0.0005 | Historical popular token snapshots               |
| `GET /chain/tokens/newest`          | $0.0005 | Recently deployed tokens                         |
| `GET /chain/trades`                 | $0.0005 | Chain-wide trade feed                            |
| `GET /chain/trades/dna`             | $0.003  | Chain-wide trades with WalletDNA                 |
| `GET /chain/transfers`              | $0.0003 | Chain-wide transfer feed                         |
| `GET /chain/transfers/dna`          | $0.001  | Chain-wide transfers with WalletDNA              |
| `GET /wallet/reputation`            | $0.0005 | Basic wallet reputation score                    |
| `GET /wallet/reputation/full`       | $0.001  | Full wallet reputation profile with DNA          |
| `GET /wallet/reputation/holder`     | $0.001  | Wallet reputation for a specific token holding   |
| `GET /wallet/trades`               | $0.0005 | Trade history for a specific wallet              |
| `GET /wallet/trades/dna`           | $0.003  | Wallet trades with DNA enrichment                |
| `GET /wallet/transfers`            | $0.0003 | Transfer history for a specific wallet           |
| `GET /wallet/transfers/dna`        | $0.001  | Wallet transfers with DNA enrichment             |
| `GET /pair/trades`                 | $0.0005 | Trading data for specific token pairs            |
| `GET /pair/trades/dna`             | $0.003  | Pair trades with WalletDNA enrichment            |

## When to Use

- **Smart money tracking** -- User wants to see what smart money wallets are buying or selling.
- **Wallet reputation** -- User wants to assess the quality or history of a specific wallet address.
- **Token trade analysis** -- User wants to see recent trades with behavioral context (who is buying and are they
  smart money).
- **Trending tokens** -- User asks what tokens are trending, popular, or newly deployed on a chain.
- **Transfer monitoring** -- User wants to track large transfers and know whether they come from smart money wallets.
- **Token holder quality** -- User wants to know the reputation breakdown of a token's holders.
- **Historical pricing** -- User needs token prices at a specific block number or timestamp.
- **Exotic pricing** -- User wants a token's price quoted in another ERC20 token instead of USD.

## Best Practices

- **Use DNA-enriched endpoints for smart money insights** -- The `/dna` variants cost more but provide wallet
  behavioral data that standard endpoints do not. Use them when smart money context matters.
- **Use `estimate_cost=true` to preview costs** -- Append this parameter to any request to see the cost without
  actually executing the query.
- **Filter by blockchain** -- Always specify the `blockchain` parameter (ethereum or base) to get accurate results.
- **Use `num` to control response size** -- Limit results with the `num` parameter (1-1000) to reduce cost and
  response size.
- **Combine with wallet reputation** -- When analyzing trades, cross-reference with `/wallet/reputation/full` for
  deeper context on specific wallets.
- **Use address over symbol when possible** -- Token addresses are unambiguous. Use `addresses` parameter instead of
  `symbols` when you have the contract address.

## Error Handling

| Error                      | Cause                                    | Solution                                                                                  |
|----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`     | Payment not processed or insufficient    | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `400 Bad Request`          | Missing or invalid query parameters      | Ensure required params (blockchain, token_address or symbols) are present.                |
| `404 Not Found`            | Token or wallet not found                | Verify the token address or wallet address exists on the specified blockchain.             |
| `422 Unprocessable Entity` | Invalid parameter combination            | Check that blockchain is "ethereum" or "base" and addresses are valid.                    |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests. Consider using `num` to reduce result size.           |
| `500 Internal Server Error`| Upstream SlamAI service issue            | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.    |
