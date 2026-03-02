---
name: obul-reddit
description: "USE THIS SKILL WHEN: the user wants to search Reddit posts, read post comments, or find discussions on specific topics. Provides pay-per-use Reddit data via StableEnrich through the Obul proxy."
homepage: https://www.reddit.com
metadata:
  obul-skill:
    emoji: "🔴"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# Reddit

Reddit provides access to Reddit post search and comment retrieval. Search for Reddit posts by keyword and retrieve full post details with comments. No Reddit API key needed — payment is handled automatically by the Obul proxy.

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

Base URL: `https://proxy.obul.ai/proxy/https/stableenrich.dev`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Search Reddit Posts

Search for Reddit posts by keyword with optional sorting and timeframe filters.

**Pricing:** $0.02

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/stableenrich.dev/api/reddit/search",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "query": "rust programming language",
    "sort": "relevance",
    "timeframe": "month",
    "maxResults": 10
  }
}
```

**Response:** JSON array of Reddit posts with truncated previews including title, subreddit, score, comment count, author, and permalink.

### Get Post Comments

Retrieve full post details and comments for a specific Reddit post.

**Pricing:** $0.02

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/stableenrich.dev/api/reddit/post-comments",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://www.reddit.com/r/programming/comments/abc123/example_post/"
  }
}
```

**Response:** JSON object with the full post details (including untruncated selftext) and a nested comment tree with author, score, and body for each comment.

## Endpoint Pricing Reference

| Endpoint                       | Price | Purpose                        |
|--------------------------------|-------|--------------------------------|
| `POST /api/reddit/search`      | $0.02 | Search Reddit posts            |
| `POST /api/reddit/post-comments` | $0.02 | Get post and comments         |

## When to Use

- **Post search** — User asks to search Reddit for posts on a topic
- **Read comments** — User wants to read comments on a Reddit post
- **Discussion research** — User asks about Reddit discussions or threads
- **Sentiment lookup** — User wants to find what Reddit thinks about something
- **Content extraction** — User provides a Reddit URL and wants the content

## Best Practices

- **Search first** — Always search first to find relevant posts, then fetch comments for the most relevant ones
- **Selftext is truncated in search** — Use post-comments to get full selftext and discussion
- **Sort options** — Use `sort`: `relevance`, `hot`, `top`, `new`, `comments`
- **Timeframe filter** — Use `timeframe`: `hour`, `day`, `week`, `month`, `year`, `all`

## Error Handling

| Error                       | Cause                                    | Solution                                                                                  |
|-----------------------------|------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai.   |
| `400 Bad Request`           | Missing or invalid request body          | Ensure required fields like `query` or `url` are present.                                  |
| `404 Not Found`             | Post not found                           | Verify the Reddit URL or search query is correct.                                          |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests.                                                       |
| `500 Internal Server Error` | Reddit service issue                     | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
