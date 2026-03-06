---
name: heyelsa
description: "USE THIS SKILL WHEN: the user wants DeFi portfolio analytics, token prices, wallet balances, swap execution, limit orders, staking positions, yield suggestions, airdrop checks, PnL reports, or gas prices via HeyElsa."
homepage: https://x402.heyelsa.ai
metadata:
  obul-skill:
    emoji: "💰"
registries: {}
---

# HeyElsa DeFi Suite

HeyElsa provides a comprehensive pay-per-use DeFi toolkit for AI agents covering portfolio analytics, token search,
real-time pricing, multi-chain wallet balances, swap execution, limit orders, staking, yield discovery, airdrop
eligibility, and gas prices. Through the Obul proxy, each request is paid individually with no HeyElsa account or API
key required.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://x402-api.heyelsa.ai`

## Common Operations

### Get Portfolio

Retrieve a comprehensive portfolio analysis for a wallet address, including token holdings, total value, and
allocation breakdown across chains.

**Pricing:** $0.01

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"wallet_address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"}' \
  "https://x402-api.heyelsa.ai/api/get_portfolio"
```

**Response:** JSON object with portfolio breakdown including token balances, USD values, allocation percentages, and
multi-chain summary.

### Get Token Price

Retrieve real-time pricing data for a token including price, market cap, volume, and 24h change.

**Pricing:** $0.002

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"token": "ETH"}' \
  "https://x402-api.heyelsa.ai/api/get_token_price"
```

**Response:** JSON object with current price, market cap, 24h volume, and price change data.

### Execute Swap

Execute a token swap across chains with optimal routing. Use `dry_run: true` to preview the swap without executing.

**Pricing:** $0.02

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"from_chain": "ethereum", "from_token": "ETH", "from_amount": "0.1", "to_chain": "ethereum", "to_token": "USDC", "wallet_address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "slippage": 0.5, "dry_run": true}' \
  "https://x402-api.heyelsa.ai/api/execute_swap"
```

**Response:** JSON object with swap quote including expected output amount, price impact, route details, and gas
estimate. When `dry_run` is false, returns the executed transaction hash.

### Get Swap Quote

Get optimal swap routing and pricing without executing the trade. Useful for comparing prices before committing.

**Pricing:** $0.01

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"from_chain": "ethereum", "from_token": "ETH", "from_amount": "1.0", "to_chain": "base", "to_token": "USDC"}' \
  "https://x402-api.heyelsa.ai/api/get_swap_quote"
```

**Response:** JSON object with optimal route, expected output, price impact, estimated gas, and fees.

### Analyze Wallet

Perform risk and behavioral analysis on a wallet address, including transaction patterns, DeFi exposure, and risk
scoring.

**Pricing:** $0.02

```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"wallet_address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"}' \
  "https://x402-api.heyelsa.ai/api/analyze_wallet"
```

**Response:** JSON object with wallet risk score, behavioral classification, transaction frequency, DeFi protocol
exposure, and historical activity summary.

## Endpoint Pricing Reference

| Endpoint                    | Price  | Purpose                                            |
|-----------------------------|--------|----------------------------------------------------|
| `POST /api/search_token`    | $0.001 | Search for tokens across all blockchains           |
| `POST /api/get_token_price` | $0.002 | Retrieve real-time token pricing                   |
| `POST /api/get_balances`    | $0.005 | Get wallet token balances across chains            |
| `POST /api/get_portfolio`   | $0.01  | Comprehensive portfolio analysis                   |
| `POST /api/analyze_wallet`  | $0.02  | Wallet risk and behavioral analysis                |
| `POST /api/get_pnl_report`  | $0.015 | Profit and loss calculations                       |
| `POST /api/get_swap_quote`  | $0.01  | Get optimal swap routing and pricing               |
| `POST /api/execute_swap`    | $0.02  | Execute blockchain swaps                           |
| `POST /api/create_limit_order` | $0.05 | Create CoW Protocol limit orders                |
| `POST /api/get_limit_orders`   | $0.002 | View pending limit orders                       |
| `POST /api/cancel_limit_order` | $0.01 | Cancel an existing limit order                  |
| `POST /api/get_stake_balances` | $0.005 | View staking positions                          |
| `POST /api/get_yield_suggestions` | $0.02 | Discover yield opportunities                 |
| `POST /api/check_airdrop`  | $0.002 | Check airdrop eligibility and allocation           |
| `POST /api/claim_airdrop`  | $0.001 | Claim ELSA tokens                                  |
| `POST /api/get_transaction_history` | $0.003 | Retrieve transaction records               |
| `POST /api/get_transaction_status`  | $0.001 | Monitor pipeline/transaction status        |
| `POST /api/submit_transaction_hash` | $0.005 | Submit signed transaction hashes           |
| `POST /api/get_gas_prices`  | $0.001 | Current gas price data across chains               |
| `GET /api/health`           | $0.00  | Server health check                                |

## When to Use

- **Portfolio overview** -- User wants to see their holdings, balances, or portfolio value across chains.
- **Token lookup** -- User asks for the price, market cap, or volume of a specific token.
- **Swap execution** -- User wants to swap tokens on-chain with optimal routing.
- **Limit orders** -- User wants to set, view, or cancel limit orders via CoW Protocol.
- **Staking and yield** -- User asks about staking positions or yield farming opportunities.
- **Wallet analysis** -- User wants risk scoring or behavioral analysis of a wallet.
- **PnL tracking** -- User wants profit and loss reports for their trading activity.
- **Airdrop checking** -- User wants to check eligibility for ELSA or other airdrops.
- **Gas prices** -- User needs current gas prices before submitting a transaction.

## Best Practices

- **Use `dry_run: true` before executing swaps** -- Always preview the swap output, price impact, and gas estimate
  before committing to a trade.
- **Search before quoting** -- Use `/api/search_token` to resolve token names to addresses before calling price or
  swap endpoints.
- **Set reasonable slippage** -- Default slippage of 0.5% works for most trades. Increase for low-liquidity tokens.
- **Check gas prices first** -- Call `/api/get_gas_prices` before executing transactions to time your trade for lower
  gas costs.
- **Use get_swap_quote for comparison** -- Compare quotes before executing to ensure optimal pricing.
- **Monitor transaction status** -- After submitting a swap, use `/api/get_transaction_status` to track completion.

## Error Handling

| Error                      | Cause                                    | Solution                                                                                  |
|----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`     | Payment not processed or insufficient    | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `400 Bad Request`          | Missing or invalid request body          | Ensure required fields (wallet_address, token, etc.) are present and correctly typed.     |
| `404 Not Found`            | Token or wallet not found                | Verify the token symbol or wallet address is correct and on a supported chain.            |
| `422 Unprocessable Entity` | Valid request but cannot process          | Check that chain names and token symbols match supported values.                          |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests and avoid rapid-fire calls.                            |
| `500 Internal Server Error`| Upstream HeyElsa service issue           | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.    |
