---
name: farcaster
description: Search Farcaster for casts, profiles, and channels. Usage: /obul-social:farcaster <query>
---

Search Farcaster using Neynar's x402 API.

## Usage

```
/obul-social:farcaster casts about onchain governance
/obul-social:farcaster user vitalik
/obul-social:farcaster channel ethereum
/obul-social:farcaster feed base
```

## Workflow

1. Ensure you are logged in via `obulx login`
2. Determine intent from the query:
   - Cast search (default): use `/v2/farcaster/cast/search`
   - User search (query starts with "user" or "@"): use `/v2/farcaster/user/search`
   - Channel search (query starts with "channel"): use `/v2/farcaster/channel/search`
   - Feed (query starts with "feed"): use `/v2/farcaster/feed` with `feed_type=filter&filter_type=channel_id`
3. Send the request through `https://api.neynar.com{path}` using obulx
4. Display results in a readable format with author, text, reactions, and timestamps
