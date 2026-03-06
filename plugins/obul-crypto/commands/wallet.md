---
name: wallet
description: Look up wallet balances and transactions. Usage: /obul-crypto:wallet <address>
---

Look up token balances and recent transactions for a blockchain wallet address.

## Usage

```
/obul-crypto:wallet 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
/obul-crypto:wallet vitalik.eth
/obul-crypto:wallet bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06
```

## Workflow

1. Ensure you are logged in via `obulx login`. If not, tell the user to run `obulx login`.

2. Detect the address type:
   - Starts with `0x` (42 chars): EVM address (Ethereum, Base, etc.)
   - Contains `.eth`: ENS name -- resolve it first
   - Starts with `bc1` or `1` or `3`: Bitcoin address

3. **If ENS name**, resolve to address first:

```bash
obulx "https://x402-gateway-production.up.railway.app/api/ens/resolve?name={ens_name}"
```

4. **If EVM address**, fetch balances and transactions:

```bash
# Balances
obulx -X POST -H "Content-Type: application/json" \
  -d '{"address": "{address}", "chain": "ethereum"}' \
  "https://x402-gateway-production.up.railway.app/api/wallet/balances"

# Recent transactions
obulx -X POST -H "Content-Type: application/json" \
  -d '{"address": "{address}", "chain": "ethereum"}' \
  "https://x402-gateway-production.up.railway.app/api/wallet/transactions"
```

5. **If Bitcoin address**, fetch UTXOs and ordinals data:

```bash
obulx "https://api.ordiscan.com/v1/address/{address}/utxos"
```

Optionally also fetch rune and BRC-20 balances:

```bash
# Runes
obulx "https://api.ordiscan.com/v1/address/{address}/runes"

# BRC-20
obulx "https://api.ordiscan.com/v1/address/{address}/brc20"
```

6. Display a summary:
   - For EVM: table of token balances (symbol, amount, USD value) and last 5 transactions
   - For Bitcoin: UTXO count, total BTC, plus any runes/BRC-20/inscriptions held

## Cost

- ENS resolve: ~$0.001
- EVM balances: ~$0.005
- EVM transactions: ~$0.005
- Bitcoin UTXOs: ~$0.01
- Runes/BRC-20: ~$0.01 each
