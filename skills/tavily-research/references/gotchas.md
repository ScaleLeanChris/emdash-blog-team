# Research Gotchas

## Credit Usage

Each search costs 1 credit (basic) or 2 credits (advanced). Each extract costs 1 credit per 5 URLs. The free tier gives 1,000 credits/month. For a blog publishing 3-5 posts/week with 2-3 searches per topic, budget ~40-60 credits/week.

## include_answer vs results

- `include_answer: true` gives a quick LLM-generated summary — useful for the "Quick Answer" section of a brief
- `results` gives the actual source URLs and snippets — always use these for citations

Don't use `include_answer` as the sole basis for content. It's a starting point, not a source.

## include_raw_content

Setting `include_raw_content: true` on search returns full article text but increases response size significantly. For initial research, use snippets first. Only extract full content from the most relevant URLs.

## Time-Sensitive Topics

Use `time_range` to scope results:
- `day` — breaking news
- `week` — recent developments
- `month` — current trends
- `year` — established content

For evergreen topics, omit `time_range` entirely to get the best overall results.

## Domain Filtering

- `include_domains` limits search to specific sites — use for competitor analysis
- `exclude_domains` skips sites — useful for avoiding your own site in results
- Max 300 include domains, 150 exclude domains

## Source Attribution

Always include source URLs in research briefs. The Content Writer needs them for:
- Internal linking to authoritative sources
- Fact-checking claims
- Avoiding plagiarism
- Building credibility with readers

## Rate Limiting

If you hit 429 (Too Many Requests), the response includes a `retry-after` header. Wait that many seconds before retrying. Don't retry in a tight loop.

## Search vs Extract

- **Search** — when you need to discover what's out there on a topic
- **Extract** — when you already have specific URLs and need the full content

Don't extract URLs you found in search unless you actually need the full text. Snippets are often enough for a research brief.
