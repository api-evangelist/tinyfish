---
name: Search the web and fetch clean content
description: Get structured web search results and extract clean markdown/JSON/HTML from URLs using the TinyFish Search and Fetch APIs.
api: openapi/tinyfish-search-openapi.json
operations: [search, fetchUrls, listSearchUsage, listFetchUsage]
---

# Search the web and fetch clean content

## Auth
Send `X-API-Key: <key>` on every request.

## Steps
1. **Search:** `GET /` on `https://api.search.tinyfish.ai` (`search`) with the query parameters. Returns ranked results with titles, snippets, and URLs as structured JSON.
2. **Fetch content:** `POST /` on `https://api.fetch.tinyfish.ai` (`fetchUrls`) with up to 10 URLs and a target `format` (markdown/JSON/HTML). Per-URL failures appear in `errors[]` and do not fail the whole request.
3. **Track usage:** `GET /usage` on each host (`listSearchUsage`, `listFetchUsage`) — cursor-paginated (`cursor`, `limit`).

## Rules
- Fetch accepts at most 10 URLs per call; batch larger sets yourself.
- Honor `Retry-After` on HTTP 429 (`RATE_LIMIT_EXCEEDED`).
- Search/Fetch access must be enabled on the account (403 `FORBIDDEN` otherwise).
- Correlate support issues with the `request_id` in the response.
