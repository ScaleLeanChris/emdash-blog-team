# Tavily API Reference

## Authentication

```
Authorization: Bearer $TAVILY_API_KEY
Content-Type: application/json
```

Get a free key at https://app.tavily.com (1,000 credits/month free, no credit card).

## Search

**Endpoint:** `POST https://api.tavily.com/search`

```json
{
  "query": "your search terms",
  "search_depth": "basic",
  "topic": "general",
  "max_results": 5,
  "include_answer": true,
  "include_raw_content": false,
  "time_range": "month",
  "include_domains": [],
  "exclude_domains": []
}
```

**Parameters:**

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `query` | string | required | Search terms |
| `search_depth` | string | `basic` | `basic` (1 credit) or `advanced` (2 credits, better relevance) |
| `topic` | string | `general` | `general` or `news` |
| `max_results` | int | 5 | 1-20 results |
| `include_answer` | bool | false | LLM-generated quick answer |
| `include_raw_content` | bool | false | Full article content as markdown |
| `time_range` | string | - | `day`, `week`, `month`, or `year` |
| `include_domains` | array | - | Only search these domains |
| `exclude_domains` | array | - | Skip these domains |

**Response:**

```json
{
  "query": "search terms",
  "answer": "Quick answer (if requested)",
  "results": [
    {
      "title": "Article Title",
      "url": "https://example.com/article",
      "content": "Snippet or summary",
      "score": 0.95
    }
  ],
  "response_time": 0.42
}
```

## Extract

**Endpoint:** `POST https://api.tavily.com/extract`

Extract full content from specific URLs:

```json
{
  "urls": ["https://example.com/article-1", "https://example.com/article-2"],
  "format": "markdown"
}
```

**Parameters:**

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `urls` | array | required | Up to 20 URLs |
| `format` | string | `markdown` | `markdown` or `text` |
| `query` | string | - | Rerank content chunks by relevance |

**Response:**

```json
{
  "results": [
    {
      "url": "https://example.com/article",
      "raw_content": "Full article content in markdown..."
    }
  ],
  "failed_results": []
}
```

**Cost:** 1 credit per 5 successful extractions.

## Rate Limits

| Tier | Search/Extract | Research |
|------|---------------|----------|
| Free | 100 RPM | 20 RPM |
| Paid | 1,000 RPM | 20 RPM |

## Pricing

- **Free:** 1,000 credits/month
- **Project ($30/mo):** 4,000 credits
- **Scale ($100/mo):** 20,000 credits
- **Overage:** $0.008/credit
