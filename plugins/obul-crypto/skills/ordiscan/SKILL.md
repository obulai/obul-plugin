---
name: obul-ordiscan
description: "USE THIS SKILL WHEN: the user wants to query Bitcoin Ordinals inscriptions, runes, BRC-20 balances, rare sats, or UTXOs. Provides pay-per-use Bitcoin data via Ordiscan through the Obul proxy."
homepage: https://ordiscan.com
metadata:
  obul-skill:
    emoji: "₿"
    requires:
      env: []
      primaryEnv: ""
registries: {}
---

# Ordiscan

Ordiscan provides pay-per-request Bitcoin Ordinals and Runes data. Query inscriptions, rune balances, BRC-20 tokens, rare sats, and UTXOs by address. No API key needed — payment is handled automatically via `obulx`.

## Authentication

All requests use the `obulx` CLI, which handles x402 payment automatically.

## Common Operations

### Get Inscription by ID

Retrieve details about a specific inscription by its ID.

**Pricing:** $0.01

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/inscription/b61b0172d95e266c18aea0c624db987e971a5d6d4ebc2aaed85da4642d635735i0"
```

**Response:** JSON with inscription details including content type, content URL, sat number, owner address, and genesis transaction.

### Get Address Inscriptions

List all inscriptions owned by a Bitcoin address.

**Pricing:** $0.01

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/inscriptions"
```

**Response:** JSON array of inscription IDs and metadata owned by the address.

### Get Address UTXOs

Retrieve UTXOs for a Bitcoin address with ordinals info.

**Pricing:** $0.01

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/utxos"
```

**Response:** JSON array of UTXOs with associated inscriptions and runes tied to each output.

### Get Rune Balances

Retrieve rune balances for a Bitcoin address.

**Pricing:** $0.01

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/runes"
```

**Response:** JSON array of rune balances in the smallest denomination (no decimals).

### Get BRC-20 Balances

Retrieve BRC-20 token balances for a Bitcoin address.

**Pricing:** $0.01

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/brc20"
```

**Response:** JSON array of BRC-20 token balances with ticker and amount.

### Get Rare Sats

Find rare sats in a Bitcoin address.

**Pricing:** $0.01

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/rare-sats"
```

**Response:** JSON array of rare sats with rarity classification and sat number.

### Get Inscription Transfers

Get inscription transfer history for a Bitcoin address.

**Pricing:** $0.01

**Request:**
```sh
obulx "https://api.ordiscan.com/v1/address/bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06/transfers"
```

**Response:** JSON array of inscription transfers for the address.

## Endpoint Pricing Reference

| Endpoint                         | Price | Purpose                              |
|----------------------------------|-------|--------------------------------------|
| `GET /v1/inscription/{id}`       | $0.01 | Get inscription by ID                |
| `GET /v1/address/{address}/inscriptions` | $0.01 | List address inscriptions |
| `GET /v1/address/{address}/utxos` | $0.01 | Get address UTXOs                    |
| `GET /v1/address/{address}/runes` | $0.01 | Get rune balances                    |
| `GET /v1/address/{address}/brc20` | $0.01 | Get BRC-20 balances                 |
| `GET /v1/address/{address}/rare-sats` | $0.01 | Get rare sats |
| `GET /v1/address/{address}/transfers` | $0.01 | Get inscription transfers |

## When to Use

- **Ordinals lookup** — User asks about Bitcoin Ordinals or inscriptions
- **Inscription ownership** — User wants to look up inscriptions owned by a Bitcoin address
- **Rune balances** — User asks about Runes token balances
- **BRC-20 tokens** — User asks about BRC-20 token balances
- **Rare sats** — User wants to find rare sats in a wallet
- **Specific inscription** — User asks about a specific inscription by ID
- **UTXO data** — User wants UTXO data for a Bitcoin address with ordinals info

## Best Practices

- **Address formats** — Bitcoin addresses can be any format: P2PKH (1...), P2SH (3...), Bech32 (bc1q...), or Taproot (bc1p...)
- **Rune denominations** — Rune balances are returned in the smallest denomination and never have decimals
- **Combine queries** — Use UTXO endpoint to get comprehensive output data including inscriptions and runes

## Notes
- All endpoints cost ~$0.01 USDC per request.
- The `obulx` CLI handles x402 payment automatically -- no USDC wallet or payment header needed.

## Error Handling

| Error                       | Cause                                    | Solution                                                                                  |
|-----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your obulx setup is correct and your account has sufficient balance at my.obul.ai.  |
| `400 Bad Request`           | Invalid address format                   | Ensure the Bitcoin address is in a valid format.                                           |
| `404 Not Found`             | Address or inscription not found         | Verify the address or inscription ID is correct.                                           |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests.                                                       |
| `500 Internal Server Error` | Ordiscan service issue                   | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
