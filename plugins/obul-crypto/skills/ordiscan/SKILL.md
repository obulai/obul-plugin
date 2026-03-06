---
name: ordiscan
description: Query Bitcoin Ordinals inscriptions, runes, BRC-20 balances, rare sats, and UTXOs via Ordiscan x402 API. Use when the user asks about Bitcoin Ordinals, inscriptions, runes, or BRC-20 tokens.
---

# ordiscan Skill

Pay-per-request Bitcoin Ordinals and Runes data from Ordiscan. Query inscriptions, rune balances, BRC-20 tokens, rare sats, and UTXOs by address.

## When to Use
- User asks about Bitcoin Ordinals or inscriptions
- User wants to look up inscriptions owned by a Bitcoin address
- User asks about Runes token balances
- User asks about BRC-20 token balances
- User wants to find rare sats in a wallet
- User asks about a specific inscription by ID
- User wants UTXO data for a Bitcoin address with ordinals info

## Endpoints

### Get Inscription by ID
- **URL**: `https://api.ordiscan.com/v1/inscription/{inscription_id}`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/inscription/b61b0172d95e266c18aea0c624db987e971a5d6d4ebc2aaed85da4642d635735i0"
```

**Response:** JSON with inscription details including content type, content URL, sat number, owner address, and genesis transaction.

### Get Address Inscriptions
- **URL**: `https://api.ordiscan.com/v1/address/{address}/inscriptions`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/inscriptions"
```

**Response:** JSON array of inscription IDs and metadata owned by the address.

### Get Address UTXOs
- **URL**: `https://api.ordiscan.com/v1/address/{address}/utxos`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/utxos"
```

**Response:** JSON array of UTXOs with associated inscriptions and runes tied to each output.

### Get Rune Balances
- **URL**: `https://api.ordiscan.com/v1/address/{address}/runes`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/runes"
```

**Response:** JSON array of rune balances in the smallest denomination (no decimals).

### Get BRC-20 Balances
- **URL**: `https://api.ordiscan.com/v1/address/{address}/brc20`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/brc20"
```

**Response:** JSON array of BRC-20 token balances with ticker and amount.

### Get Rare Sats
- **URL**: `https://api.ordiscan.com/v1/address/{address}/rare-sats`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/rare-sats"
```

**Response:** JSON array of rare sats with rarity classification and sat number.

### Get Inscription Transfers
- **URL**: `https://api.ordiscan.com/v1/address/{address}/transfers`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/transfers"
```

**Response:** JSON array of inscription transfer events with from/to addresses and transaction IDs.

## Notes
- All endpoints cost ~$0.01 USDC per request.
- Bitcoin addresses can be any format: P2PKH (1...), P2SH (3...), Bech32 (bc1q...), or Taproot (bc1p...).
- Rune balances are returned in the smallest denomination and never have decimals.
- The `obulx` CLI handles x402 payment automatically -- no USDC wallet or payment header needed.
