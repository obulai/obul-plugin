---
name: reddit
description: Search Reddit posts and fetch post comments via StableEnrich (enrichx402). Use when users ask about Reddit discussions, want to find posts, or read comment threads.
---

# reddit Skill

Access Reddit data through StableEnrich's x402-enabled API. Search for Reddit posts by keyword and retrieve full post details with comments.

## When to Use
- User asks to search Reddit for posts on a topic
- User wants to read comments on a Reddit post
- User asks about Reddit discussions or threads
- User wants to find what Reddit thinks about something
- User provides a Reddit URL and wants the content

## Endpoints

### Search Reddit Posts
- **URL**: `https://stableenrich.dev/api/reddit/search`
- **Method**: POST
- **Pricing**: ~$0.02 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"query": "rust programming language", "sort": "relevance", "timeframe": "month", "maxResults": 10}' \
  "https://stableenrich.dev/api/reddit/search"
```

**Parameters:**
- `query` (required) -- search query string
- `sort` -- sort order: `relevance`, `hot`, `top`, `new`, `comments`
- `timeframe` -- time filter: `hour`, `day`, `week`, `month`, `year`, `all`
- `maxResults` -- number of results to return

**Response:** JSON array of Reddit posts with truncated previews including title, subreddit, score, comment count, author, and permalink. Selftext is truncated for efficiency; use `post-comments` to get full text.

### Get Post Comments
- **URL**: `https://stableenrich.dev/api/reddit/post-comments`
- **Method**: POST
- **Pricing**: ~$0.02 per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"url": "https://www.reddit.com/r/programming/comments/abc123/example_post/"}' \
  "https://stableenrich.dev/api/reddit/post-comments"
```

**Parameters:**
- `url` (required) -- full Reddit post permalink URL

**Response:** JSON object with the full post details (including untruncated selftext) and a nested comment tree with author, score, and body for each comment.

## Notes
- Search returns truncated previews; always use `post-comments` to get full selftext and discussion
- Typical workflow: search first to find relevant posts, then fetch comments for the most relevant ones
- Both endpoints are POST requests and require a JSON body
- Pricing is $0.02 per request for both endpoints
- Results include standard Reddit metadata: subreddit, score, author, timestamps, and permalinks
