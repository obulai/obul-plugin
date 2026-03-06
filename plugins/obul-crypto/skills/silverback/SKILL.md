---
name: silverback
description: "USE THIS SKILL WHEN: the user wants DeFi analytics including technical analysis (RSI, MACD, Bollinger Bands), arbitrage scanning, DeFi yield finding, whale monitoring, token security audits, strategy backtesting, liquidity pool analysis, or multi-chain gas prices."
homepage: https://x402.silverbackdefi.app
metadata:
  obul-skill:
    emoji: "💰"
registries: {}
---

# Silverback DeFi Analytics

Silverback provides comprehensive DeFi analytics for AI agents, covering technical analysis indicators (RSI, MACD,
Bollinger Bands), cross-DEX arbitrage scanning, DeFi yield discovery, whale wallet monitoring, token security audits
with honeypot detection, strategy backtesting, liquidity pool analysis, and multi-chain gas prices. Through the Obul
proxy, each request is paid individually with no Silverback account or API key required.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://x402.silverbackdefi.app`

## Common Operations

### Technical Analysis

Retrieve technical analysis indicators for a token including RSI, MACD, Bollinger Bands, moving averages, and trend
signals. Supports multiple timeframes.

**Pricing:** $0.005

```sh
obulx "https://x402.silverbackdefi.app/api/v1/technical-analysis?token=ETH&timeframe=4h&chain=ethereum"
```

**Response:** JSON object with RSI value and signal (overbought/oversold), MACD line, signal line, and histogram,
Bollinger Bands (upper, middle, lower), moving averages (SMA, EMA), and overall trend direction.

### Arbitrage Scanner

Scan for arbitrage opportunities across DEXes on a specific chain. Finds price discrepancies for profitable
cross-DEX trades.

**Pricing:** $0.01

```sh
obulx "https://x402.silverbackdefi.app/api/v1/arbitrage?chain=ethereum&min_profit_pct=0.5"
```

**Response:** JSON array of arbitrage opportunities with token pair, buy DEX, sell DEX, price difference percentage,
estimated profit, and required gas cost.

### Token Audit

Perform a security audit on a token contract, checking for honeypot behavior, mint functions, hidden fees, ownership
renouncement, liquidity locks, and other risk factors.

**Pricing:** $0.01

```sh
obulx "https://x402.silverbackdefi.app/api/v1/token-audit?token_address=0x6982508145454Ce325dDbE47a25d4ec3d2311933&chain=ethereum"
```

**Response:** JSON object with audit results including honeypot detection, mint function status, hidden fee analysis,
ownership status, liquidity lock info, and an overall risk score.

### DeFi Yield Finder

Discover the best DeFi yield opportunities across protocols, filterable by chain, asset, and minimum TVL.

**Pricing:** $0.005

```sh
obulx "https://x402.silverbackdefi.app/api/v1/yields?chain=ethereum&asset=USDC&min_tvl=1000000"
```

**Response:** JSON array of yield opportunities with protocol name, pool, APY, TVL, risk rating, and reward token
details.

### Whale Monitor

Track large wallet movements and whale activity for a specific token, including accumulation and distribution
patterns.

**Pricing:** $0.01

```sh
obulx "https://x402.silverbackdefi.app/api/v1/whale-monitor?token=ETH&chain=ethereum&min_usd=100000"
```

**Response:** JSON object with recent whale transactions, top holders, concentration metrics (Gini coefficient),
and accumulation/distribution trend signals.

## Endpoint Pricing Reference

| Endpoint                                 | Price  | Purpose                                              |
|------------------------------------------|--------|------------------------------------------------------|
| `GET /api/v1/technical-analysis`         | $0.005 | RSI, MACD, Bollinger Bands, moving averages          |
| `GET /api/v1/technical-analysis/summary` | $0.003 | Quick trend summary with buy/sell signal              |
| `GET /api/v1/technical-analysis/rsi`     | $0.002 | RSI indicator only                                   |
| `GET /api/v1/technical-analysis/macd`    | $0.002 | MACD indicator only                                  |
| `GET /api/v1/technical-analysis/bollinger` | $0.002 | Bollinger Bands only                               |
| `GET /api/v1/arbitrage`                  | $0.01  | Cross-DEX arbitrage opportunity scanner              |
| `GET /api/v1/arbitrage/pairs`            | $0.005 | Arbitrage for specific token pair                    |
| `GET /api/v1/yields`                     | $0.005 | DeFi yield finder across protocols                   |
| `GET /api/v1/yields/top`                 | $0.003 | Top yielding pools by APY                            |
| `GET /api/v1/whale-monitor`              | $0.01  | Whale activity and large transaction tracking        |
| `GET /api/v1/whale-monitor/alerts`       | $0.005 | Recent whale movement alerts                         |
| `GET /api/v1/whale-monitor/holders`      | $0.005 | Top holders and concentration analysis               |
| `GET /api/v1/token-audit`                | $0.01  | Token security audit with honeypot detection         |
| `GET /api/v1/token-audit/quick`          | $0.003 | Quick safety check (honeypot + liquidity only)       |
| `GET /api/v1/backtest`                   | $0.05  | Strategy backtesting with historical data            |
| `GET /api/v1/backtest/signals`           | $0.02  | Historical signal generation for a strategy          |
| `GET /api/v1/pools`                      | $0.005 | Liquidity pool analysis across DEXes                 |
| `GET /api/v1/pools/top`                  | $0.003 | Top pools by volume or TVL                           |
| `GET /api/v1/pools/new`                  | $0.003 | Recently created liquidity pools                     |
| `GET /api/v1/gas`                        | $0.001 | Multi-chain gas prices (slow, normal, fast)          |
| `GET /api/v1/gas/history`                | $0.002 | Historical gas price data                            |
| `GET /api/v1/price-feed`                 | $0.001 | Token price feed with top movers and losers          |
| `GET /api/v1/funding-rates`              | $0.005 | Perpetual funding rates across exchanges             |
| `GET /api/v1/funding-rates/arb`          | $0.008 | Funding rate arbitrage opportunities                 |
| `GET /api/v1/dex-quotes`                 | $0.002 | DEX swap quotes across protocols                     |
| `GET /api/v1/wallet-profiler`            | $0.008 | Wallet portfolio, positions, and risk classification |
| `GET /api/v1/market-overview`            | $0.003 | Overall market summary with fear/greed index         |
| `GET /api/v1/correlations`               | $0.005 | Token price correlation matrix                       |
| `GET /api/v1/volume-analysis`            | $0.005 | Volume profile and anomaly detection                 |
| `GET /api/v1/support-resistance`         | $0.003 | Key support and resistance levels                    |
| `GET /api/v1/token-metrics`              | $0.003 | On-chain token metrics (velocity, NVT ratio)         |
| `GET /api/v1/liquidations`               | $0.005 | Liquidation heatmap and recent liquidations          |
| `GET /api/v1/sentiment`                  | $0.003 | Social sentiment analysis for a token                |
| `GET /api/v1/health`                     | $0.00  | Server health check                                  |

## When to Use

- **Technical analysis** -- User asks for RSI, MACD, Bollinger Bands, or trend direction for a token.
- **Arbitrage hunting** -- User wants to find price discrepancies across DEXes for profitable trades.
- **Yield farming** -- User asks for the best DeFi yields for a specific asset or chain.
- **Whale tracking** -- User wants to monitor large wallet movements or see top holders.
- **Token safety** -- User asks whether a token is safe, wants honeypot detection, or contract audit.
- **Strategy testing** -- User wants to backtest a trading strategy against historical data.
- **Pool analysis** -- User wants to find top liquidity pools or analyze pool metrics.
- **Gas optimization** -- User needs gas prices to time transactions for lower costs.
- **Funding rates** -- User wants to see perpetual funding rates or funding rate arbitrage.
- **Market overview** -- User asks for overall market conditions, fear/greed, or correlation data.

## Best Practices

- **Start with quick checks** -- Use `/token-audit/quick` and `/technical-analysis/summary` for fast screening before
  committing to deeper (and more expensive) analysis.
- **Combine technical and on-chain data** -- Use technical analysis alongside whale monitoring and volume analysis for
  more informed trading decisions.
- **Filter yields by TVL** -- Always set a `min_tvl` when searching for yields to avoid low-liquidity pools that may
  carry higher risk.
- **Set minimum profit for arbitrage** -- Use `min_profit_pct` to filter out marginal opportunities that would not
  cover gas costs.
- **Use gas endpoint before trading** -- Check gas prices before executing swaps or arbitrage to ensure profitability.
- **Backtest before deploying** -- Use the backtesting endpoint to validate strategies on historical data before
  risking real capital.

## Error Handling

| Error                      | Cause                                    | Solution                                                                                  |
|----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`     | Payment not processed or insufficient    | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `400 Bad Request`          | Missing or invalid query parameters      | Ensure required params (token, chain, token_address) are present and correctly typed.     |
| `404 Not Found`            | Token or pool not found                  | Verify the token symbol/address exists on the specified chain.                            |
| `422 Unprocessable Entity` | Invalid parameter combination            | Check that chain name and timeframe values are supported.                                 |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests and avoid rapid-fire calls.                            |
| `500 Internal Server Error`| Upstream Silverback service issue        | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.    |
| `503 Service Unavailable`  | Silverback service temporarily down      | Retry after a brief wait.                                                                 |
