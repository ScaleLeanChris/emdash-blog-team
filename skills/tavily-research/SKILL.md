---
name: tavily-research
description: Research topics using the Tavily search API — web search, article extraction, and competitor analysis for content creation. Use when asked to research a topic, find sources, gather data, analyze competitors, find supporting evidence, or prepare a research brief. Also use when the task mentions "research," "find sources," "competitor analysis," "what are people writing about," "supporting data," "fact check," or "content gap analysis."
metadata:
  version: 1.0.0
---

# Tavily Research

Search the web and extract article content using the Tavily API. Returns structured results with titles, URLs, snippets, and optionally full article content.

## Prerequisites

- `TAVILY_API_KEY` env var must be set (get free key at https://app.tavily.com)

## Quick Start

Search the web:

```bash
curl -s -X POST "https://api.tavily.com/search" \
  -H "Authorization: Bearer $TAVILY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "best practices for blog content strategy 2026",
    "max_results": 5,
    "include_answer": true
  }' | python3 -c "
import sys, json
r = json.load(sys.stdin)
if 'answer' in r: print('Answer:', r['answer'][:300], '\n')
for i, result in enumerate(r.get('results', []), 1):
    print(f'{i}. {result[\"title\"]}')
    print(f'   {result[\"url\"]}')
    print(f'   {result[\"content\"][:150]}')
    print()
"
```

## Key Endpoints

| Endpoint | What it does | Credits |
|----------|-------------|---------|
| `POST /search` | Web search with snippets + optional full content | 1-2 per search |
| `POST /extract` | Extract full article from URLs | 1 per 5 URLs |

For full API reference, see `references/api.md`.
For research workflows, see `references/patterns.md`.
