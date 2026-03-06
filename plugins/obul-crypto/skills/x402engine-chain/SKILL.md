---
name: x402engine-chain
description: Query wallet balances, transactions, PnL, ENS resolution, token prices, and simulate transactions via x402engine. Use when the user asks about wallet holdings, transaction history, ENS names, or tx simulation.
---

# x402engine-chain Skill

Pay-per-call blockchain operations from x402engine. Covers wallet analytics, ENS resolution, token metadata, and transaction simulation.

## When to Use
- User asks for a wallet's token balances or holdings
- User asks for a wallet's transaction history
- User wants to check wallet PnL (profit and loss)
- User wants to resolve an ENS name to an address (or reverse)
- User asks for token prices or metadata
- User wants to simulate a transaction before sending
- User asks about crypto prices, trending coins, or market search

## Endpoints

### Wallet Balances
- **URL**: `https://x402-gateway-production.up.railway.app/api/wallet/balances`
- **Method**: POST
- **Pricing**: ~$0.005 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "chain": "ethereum"}' \
  "https://x402-gateway-production.up.railway.app/api/wallet/balances"
```

**Response:** JSON with token balances, USD values, and total portfolio value.

### Wallet Transactions
- **URL**: `https://x402-gateway-production.up.railway.app/api/wallet/transactions`
- **Method**: POST
- **Pricing**: ~$0.005 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "chain": "ethereum"}' \
  "https://x402-gateway-production.up.railway.app/api/wallet/transactions"
```

**Response:** JSON array of recent transactions with hash, from, to, value, and timestamp.

### Wallet PnL
- **URL**: `https://x402-gateway-production.up.railway.app/api/wallet/pnl`
- **Method**: POST
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"}' \
  "https://x402-gateway-production.up.railway.app/api/wallet/pnl"
```

**Response:** JSON with realized/unrealized PnL breakdown per token.

### ENS Resolve (Name to Address)
- **URL**: `https://x402-gateway-production.up.railway.app/api/ens/resolve`
- **Method**: GET
- **Pricing**: ~$0.001 per request

**Request:**
```sh
obulx "https://x402-gateway-production.up.railway.app/api/ens/resolve?name=vitalik.eth"
```

**Response:** JSON with the resolved Ethereum address.

### ENS Reverse (Address to Name)
- **URL**: `https://x402-gateway-production.up.railway.app/api/ens/reverse`
- **Method**: GET
- **Pricing**: ~$0.001 per request

**Request:**
```sh
obulx "https://x402-gateway-production.up.railway.app/api/ens/reverse?address=0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
```

**Response:** JSON with the ENS name associated with the address.

### Token Prices (Batch)
- **URL**: `https://x402-gateway-production.up.railway.app/api/token/prices`
- **Method**: POST
- **Pricing**: ~$0.005 per request

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
- **URL**: `https://x402-gateway-production.up.railway.app/api/tx/simulate`
- **Method**: POST
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"from": "0x...", "to": "0x...", "value": "1000000000000000000", "data": "0x...", "chain": "ethereum"}' \
  "https://x402-gateway-production.up.railway.app/api/tx/simulate"
```

**Response:** JSON with simulation results including gas estimate, state changes, and potential errors.

## Notes
- Prices range from $0.001 to $0.01 per request depending on endpoint complexity.
- Wallet endpoints accept EVM addresses and support multiple chains (ethereum, base, polygon, arbitrum, etc.).
- ENS endpoints are Ethereum-specific.
- The `obulx` CLI handles x402 payment automatically -- no USDC wallet or payment header needed.
