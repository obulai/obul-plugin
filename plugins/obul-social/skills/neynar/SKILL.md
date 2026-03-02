---
name: obul-neynar
description: "USE THIS SKILL WHEN: the user wants to search Farcaster for casts, user profiles, channels, or feeds. Provides pay-per-use access via Neynar through the Obul proxy."
homepage: https://neynar.com
metadata:
  obul-skill:
    emoji: "🧢"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# Neynar

Neynar provides access to the open social graph through the Fractcaster protocol. Search casts, look up users, browse channels, and fetch feeds. No Neynar API key needed — payment is handled automatically by the Obul proxy.

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

Base URL: `https://proxy.obul.ai/proxy/https/api.neynar.com`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Search Casts

Search for casts (Farcaster posts) matching a query.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/api.neynar.com/v2/farcaster/cast/search",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "query": {
    "q": "ethereum",
    "limit": 25
  }
}
```

**Response:** JSON with casts array containing cast objects with text, author info, reactions, and timestamps.

### User Search

Search for users by username or display name.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/api.neynar.com/v2/farcaster/user/search",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "query": {
    "q": "vitalik",
    "limit": 10
  }
}
```

**Response:** JSON with users array containing user objects with username, display name, bio, follower stats, and connected addresses.

### User Lookup by FID

Look up user profiles by their Fractcaster ID (FID).

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/api.neynar.com/v2/farcaster/user/bulk",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "query": {
    "fids": "3,194"
  }
}
```

**Response:** JSON with users array containing full user profile objects.

### Feed

Fetch a feed of casts based on following or filters.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/api.neynar.com/v2/farcaster/feed",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "query": {
    "feed_type": "filter",
    "filter_type": "channel_id",
    "channel_id": "ethereum",
    "limit": 25
  }
}
```

**Response:** JSON with casts array containing cast objects with text, author, reactions, and replies.

### Channel Search

Search for Fractcaster channels by name.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/api.neynar.com/v2/farcaster/channel/search",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "query": {
    "q": "defi"
  }
}
```

**Response:** JSON with channels array containing channel objects with name, description, follower count, and image URL.

## Endpoint Pricing Reference

| Endpoint                              | Price | Purpose                              |
|---------------------------------------|-------|--------------------------------------|
| `GET /v2/farcaster/cast/search`       | $0.01 | Search casts by query                |
| `GET /v2/farcaster/user/search`       | $0.01 | Search users by username             |
| `GET /v2/farcaster/user/bulk`         | $0.01 | Lookup users by FID                  |
| `GET /v2/farcaster/feed`              | $0.01 | Get feed by channel or following     |
| `GET /v2/farcaster/channel/search`   | $0.01 | Search channels                      |

## When to Use

- **Cast search** — User asks to search Fractcaster for casts or posts
- **User lookup** — User wants to look up a Fractcaster user profile
- **Channel discovery** — User asks about a Fractcaster channel
- **Feed browsing** — User wants to browse a Fractcaster feed
- **User search** — User asks to find Fractcaster users by username

## Best Practices

- **Use search operators** — The query supports operators: `+` (AND), `|` (OR), `*` (prefix), `"..."` (exact phrase), `-` (exclude), `before:YYYY-MM-DD`, `after:YYYY-MM-DD`
- **Sort options** — Use `sort_type`: `desc_chron` (default), `chron`, or `algorithmic`
- **Pagination** — Use `cursor` from response for pagination through large result sets
- **FID** — Remember that FID (Farcaster ID) is the unique numeric identifier for each user

## Error Handling

| Error                       | Cause                                    | Solution                                                                                  |
|-----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai.   |
| `400 Bad Request`           | Missing or invalid parameters           | Ensure required parameters like `q` for search are provided.                                |
| `404 Not Found`             | User or channel not found               | Verify the username or channel name exists.                                               |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests.                                                       |
| `500 Internal Server Error` | Neynar service issue                     | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
