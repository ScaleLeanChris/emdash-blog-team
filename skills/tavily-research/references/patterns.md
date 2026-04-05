# Research Patterns

## Topic Research Brief

When assigned a topic to research, produce a structured brief:

### Step 1: Broad search

```bash
curl -s -X POST "https://api.tavily.com/search" \
  -H "Authorization: Bearer $TAVILY_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"query\": \"${TOPIC}\",
    \"max_results\": 10,
    \"include_answer\": true,
    \"search_depth\": \"advanced\"
  }"
```

### Step 2: Extract top articles

Take the top 3-5 URLs from search results and extract full content:

```bash
curl -s -X POST "https://api.tavily.com/extract" \
  -H "Authorization: Bearer $TAVILY_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"urls\": [\"url1\", \"url2\", \"url3\"],
    \"format\": \"markdown\",
    \"query\": \"${TOPIC}\"
  }"
```

### Step 3: Compile the brief

Structure the research as:

```markdown
## Research Brief: {Topic}

### Quick Answer
{Tavily's include_answer response}

### Key Sources
1. {Title} — {URL}
   Key points: ...
2. {Title} — {URL}
   Key points: ...

### Key Findings
- Finding 1 (with source)
- Finding 2 (with source)
- Finding 3 (with source)

### Data Points
- Stat 1 (source)
- Stat 2 (source)

### Content Angles
- Angle 1: why this is interesting for {audience}
- Angle 2: contrarian take based on findings
- Angle 3: practical how-to based on evidence

### Gaps in Existing Coverage
- What competitors miss
- What questions remain unanswered
```

Post this brief as a comment on the Paperclip issue.

## Competitor Content Analysis

Research what competitors are writing about a topic:

```bash
curl -s -X POST "https://api.tavily.com/search" \
  -H "Authorization: Bearer $TAVILY_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"query\": \"${TOPIC} blog\",
    \"max_results\": 10,
    \"include_domains\": [\"competitor1.com\", \"competitor2.com\"],
    \"include_raw_content\": true
  }"
```

Then analyze:
- What topics do they cover?
- What's their angle/framing?
- What do they miss that we could cover?
- What data/sources do they cite?

## Fact Checking

Verify claims in a draft post:

```bash
curl -s -X POST "https://api.tavily.com/search" \
  -H "Authorization: Bearer $TAVILY_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"query\": \"${CLAIM_TO_VERIFY}\",
    \"max_results\": 5,
    \"search_depth\": \"advanced\",
    \"time_range\": \"year\"
  }"
```

Report whether sources support or contradict the claim.

## Trending Topics Discovery

Find what's being discussed right now:

```bash
curl -s -X POST "https://api.tavily.com/search" \
  -H "Authorization: Bearer $TAVILY_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"query\": \"${CONTENT_PILLAR} trends news\",
    \"topic\": \"news\",
    \"max_results\": 10,
    \"time_range\": \"week\"
  }"
```

Use news results to identify timely content opportunities.

## SEO Keyword Research Support

Find what content exists for a target keyword:

```bash
curl -s -X POST "https://api.tavily.com/search" \
  -H "Authorization: Bearer $TAVILY_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"query\": \"${KEYWORD}\",
    \"max_results\": 10,
    \"search_depth\": \"advanced\"
  }"
```

Analyze the top results to understand:
- What format ranks (listicles, guides, tutorials)?
- What word count / depth is typical?
- What subtopics do top results cover?
- What can we do better or differently?
