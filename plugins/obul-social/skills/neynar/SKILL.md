---
name: neynar
description: Search Farcaster for casts, user profiles, channels, and feeds via Neynar API. Use when users ask about Farcaster content or want to search the Farcaster social network.
---

# neynar Skill

Access Farcaster protocol data through Neynar's x402-enabled API. Search casts, look up users, browse channels, and fetch feeds without needing a Neynar API key -- the Obul proxy handles x402 payments automatically.

## When to Use
- User asks to search Farcaster for casts or posts
- User wants to look up a Farcaster user profile
- User asks about a Farcaster channel
- User wants to browse a Farcaster feed
- User asks to find Farcaster users by username

## Endpoints

### Search Casts
- **URL**: `https://api.neynar.com/v2/farcaster/cast/search`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.neynar.com/v2/farcaster/cast/search?q=ethereum&limit=25"
```

**Query Parameters:**
- `q` (required) -- search query string. Supports operators: `+` (AND), `|` (OR), `*` (prefix), `"..."` (exact phrase), `-` (exclude), `before:YYYY-MM-DD`, `after:YYYY-MM-DD`
- `mode` -- `literal` (default), `semantic`, or `hybrid`
- `sort_type` -- `desc_chron` (default), `chron`, or `algorithmic`
- `author_fid` -- filter by author's Farcaster ID
- `channel_id` -- filter by channel
- `parent_url` -- filter by parent URL
- `limit` -- results per page (1-100, default 25)
- `cursor` -- pagination cursor

**Response:** JSON with `result.casts` array containing cast objects with text, author info, reactions, and timestamps.

### User Search
- **URL**: `https://api.neynar.com/v2/farcaster/user/search`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.neynar.com/v2/farcaster/user/search?q=vitalik&limit=10"
```

**Query Parameters:**
- `q` (required) -- username search query
- `limit` -- results per page (1-10, default 5)
- `viewer_fid` -- FID for viewer context

**Response:** JSON with `result.users` array containing user objects with username, display name, bio, follower stats, and connected addresses.

### User Lookup by FID
- **URL**: `https://api.neynar.com/v2/farcaster/user/bulk`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.neynar.com/v2/farcaster/user/bulk?fids=3,194"
```

**Query Parameters:**
- `fids` (required) -- comma-separated list of FIDs (up to 100)
- `viewer_fid` -- FID for viewer context

**Response:** JSON with `users` array containing full user profile objects.

### Feed
- **URL**: `https://api.neynar.com/v2/farcaster/feed`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.neynar.com/v2/farcaster/feed?feed_type=filter&filter_type=channel_id&channel_id=ethereum&limit=25"
```

**Query Parameters:**
- `feed_type` -- `following` or `filter`
- `filter_type` -- `fids`, `parent_url`, or `channel_id`
- `channel_id` -- channel to filter by (when `filter_type=channel_id`)
- `fid` -- user FID (for `following` feed type)
- `limit` -- results per page (1-100, default 25)
- `cursor` -- pagination cursor

**Response:** JSON with `casts` array containing cast objects with text, author, reactions, and replies.

### Channel Search
- **URL**: `https://api.neynar.com/v2/farcaster/channel/search`
- **Method**: GET
- **Pricing**: ~$0.01 per request

**Request:**
```sh
obulx "https://api.neynar.com/v2/farcaster/channel/search?q=defi"
```

**Query Parameters:**
- `q` (required) -- channel name search query

**Response:** JSON with `channels` array containing channel objects with name, description, follower count, and image URL.

## Notes
- Neynar's x402 mode activates when no `x-api-key` header is present; the Obul proxy handles payment automatically
- All endpoints are GET requests; no Content-Type header needed
- Pricing is approximately $0.01 per request across all endpoints
- Use `cursor` from response for pagination through large result sets
- FID (Farcaster ID) is the unique numeric identifier for each Farcaster user
