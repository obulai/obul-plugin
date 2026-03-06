---
description: Search X/Twitter for tweets matching a query via twit.sh ($0.0025–$0.01/request)
---

# X/Twitter Search

Search X/Twitter for tweets matching the given query using twit.sh. Supports rich filters including keywords, exact phrases, user filters, date ranges, and engagement thresholds.

**Cost:** $0.01 per request

1. Ensure you are logged in via `obulx login`
2. URL-encode the query from `$ARGUMENTS`, then run this command:

```sh
obulx "https://x402.twit.sh/tweets/search?words=$ARGUMENTS"
```

If the user wants tweets from a specific user, use the `from` parameter:

```sh
obulx "https://x402.twit.sh/tweets/search?from=username"
```

Parse the JSON response and present the tweets to the user. Each tweet includes text, author info, engagement metrics (likes, retweets, replies, quotes, bookmarks), and timestamps. If `meta.next_token` is present, more results are available.
