---
name: obul-x-search
description: "USE THIS SKILL WHEN: the user wants to search X/Twitter, look up a Twitter user profile, read a specific tweet, browse a user's timeline, search users, or explore communities. Provides pay-per-use X/Twitter data via twit.sh through the Obul proxy."
homepage: https://twit.sh
metadata:
  obul-skill:
    emoji: "🔍"
registries: {}
---

# twit.sh

twit.sh provides pay-per-use access to X/Twitter data — full-archive tweet search with rich filters, user lookups,
user search, timelines, replies, quote tweets, followers/following, and community data. Returns X v2 API-compatible
JSON. No X/Twitter API key or developer account required — each request is paid individually through x402.

## Authentication

All requests use the `obulx` CLI, which handles proxy routing and authentication automatically.

Install and log in (one-time setup):

```sh
npm install -g @obul.ai/obulx
obulx login
```

Base URL: `https://x402.twit.sh`

## Common Operations

### Search Tweets

Full-archive tweet search with rich filtering. At least one filter parameter is required.

**Pricing:** $0.01

```sh
obulx "https://x402.twit.sh/tweets/search?words=bitcoin&minLikes=100"
```

**Parameters:**

| Parameter | Type | Description |
|---|---|---|
| `words` | string | All of these words (AND logic) |
| `phrase` | string | Exact phrase match |
| `anyWords` | string | Any of these words (OR logic) |
| `noneWords` | string | Exclude these words |
| `hashtags` | string | Filter by hashtags |
| `from` | string | From these accounts |
| `to` | string | In reply to these accounts |
| `mentioning` | string | Mentioning these accounts |
| `minReplies` | number | Minimum reply count |
| `minLikes` | number | Minimum like count |
| `minReposts` | number | Minimum retweet count |
| `since` | string | Start date (YYYY-MM-DD) |
| `until` | string | End date (YYYY-MM-DD) |
| `next_token` | string | Pagination cursor |

**Response:** Array of tweet objects with text, author info, engagement metrics (likes, retweets, replies, quotes,
bookmarks), timestamps, and entities. Includes `next_token` in `meta` for pagination.

### Look Up User by Username

Fetch a public X/Twitter profile by username, including bio, follower counts, and verification status.

**Pricing:** $0.005

```sh
obulx "https://x402.twit.sh/users/by/username?username=elonmusk"
```

**Response:** JSON object with user ID, username, display name, bio, follower/following counts, tweet count,
profile image URL, and verification status.

### Search Users

Search for X/Twitter users matching a query.

**Pricing:** $0.01

```sh
obulx "https://x402.twit.sh/users/search?query=coinbase"
```

**Response:** Array of user objects matching the query, each with username, display name, bio, and verification status.

### Get Tweet by ID

Retrieve a single tweet by its ID, including full text, author info, and engagement metrics.

**Pricing:** $0.0025

```sh
obulx "https://x402.twit.sh/tweets/by/id?id=1234567890"
```

**Response:** JSON object with the tweet's full text, author details, engagement metrics, timestamp, and any
media attachments.

### Get User Tweets

Fetch recent tweets from a specific user by their user ID.

**Pricing:** $0.01

```sh
obulx "https://x402.twit.sh/tweets/user?id=44196397"
```

**Response:** Array of the user's recent tweets, each with text, engagement metrics, and timestamps. Use `next_token`
for pagination.

## Endpoint Pricing Reference

| Endpoint                      | Price   | Purpose                                  |
|-------------------------------|---------|------------------------------------------|
| `GET /tweets/search`          | $0.01   | Full-archive tweet search with filters   |
| `GET /tweets/by/id`           | $0.0025 | Read a single tweet by ID                |
| `GET /tweets`                 | $0.01   | Retrieve multiple tweets by IDs          |
| `GET /tweets/user`            | $0.01   | List tweets by a user ID                 |
| `GET /tweets/quote_tweets`    | $0.01   | List quote tweets of a post              |
| `GET /tweets/retweeted_by`    | $0.01   | List users who reposted a tweet          |
| `GET /tweets/replies`         | $0.01   | List replies to a tweet                  |
| `GET /users/by/username`      | $0.005  | Look up user by username                 |
| `GET /users/by/id`            | $0.005  | Look up user by ID                       |
| `GET /users`                  | $0.01   | Look up multiple users by IDs            |
| `GET /users/search`           | $0.01   | Search users by query                    |
| `GET /users/followers`        | $0.01   | List followers of a user                 |
| `GET /users/following`        | $0.01   | List accounts followed by a user         |
| `GET /communities/by/id`      | $0.0025 | Community details by ID                  |
| `GET /communities/posts`      | $0.01   | Top posts from a community               |
| `GET /communities/members`    | $0.01   | List community members                   |

## When to Use

- **Full-archive tweet search** — Search tweets with rich filters (keywords, phrases, date ranges, engagement
  thresholds, specific users) across the full Twitter archive.
- **User research** — Look up public profiles by username or search for users by keyword.
- **Individual tweet lookup** — Read a specific tweet's full text and engagement metrics by ID.
- **Timeline monitoring** — Track what specific accounts are posting by fetching their recent tweets.
- **Engagement analysis** — Find high-engagement tweets using `minLikes`, `minReposts`, and `minReplies` filters.
- **Thread and reply analysis** — Fetch replies to a tweet or quote tweets for conversation context.
- **Community monitoring** — Browse community posts and member lists.
- **Social graph exploration** — List followers and following for a user.

## Best Practices

- **Use specific filters** — Combine `words`, `from`, `since`/`until`, and engagement minimums for precise results.
  At least one filter parameter is required.
- **Paginate with next_token** — When results include a `next_token` in `meta`, pass it as a query parameter to
  fetch the next page (~20 tweets per page).
- **Look up user ID first** — Some endpoints require user IDs instead of usernames. Use `/users/by/username` to
  resolve a username to an ID, then use that ID for `/tweets/user`, `/users/followers`, etc.
- **Use date ranges for historical search** — Combine `since` and `until` parameters to search specific time
  windows instead of scanning all results.
- **Cache user lookups** — User profiles change infrequently. Cache `/users/by/username` results to avoid
  repeated lookups at $0.005 each.

## Error Handling

| Error                       | Cause                                       | Solution                                                                                  |
|-----------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient       | Verify your account has sufficient balance at my.obul.ai. Run `obulx login` if not authenticated. |
| `400 Bad Request`           | Missing or invalid query parameters         | Ensure at least one search filter is provided. Check parameter names and types.           |
| `404 Not Found`             | Invalid username or tweet ID                | Double-check the username exists or the tweet ID is correct and the tweet is still public. |
| `429 Too Many Requests`     | Rate limit exceeded                         | Add a short delay between requests and avoid unnecessary rapid-fire calls.                |
| `500 Internal Server Error` | Upstream twit.sh service issue              | Wait a few seconds and retry. If persistent, check twit.sh status.                        |
