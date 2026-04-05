---
name: dataforseo
description: Query the DataForSEO API for SEO data including keyword research, SERP analysis, backlink data, and competitor research. Use when asked about keyword rankings, search volumes, SERP features, backlinks, or SEO competitive analysis.
---

# DataForSEO Skill

DataForSEO provides SEO data via REST API using HTTP Basic Auth (login + password).

## Authentication

Credentials are stored in environment variables:
- `DATAFORSEO_LOGIN` — your DataForSEO login email
- `DATAFORSEO_PASSWORD` — your DataForSEO API password

If those env vars are not set, tell the user to configure them (see setup below).

## API Base URL

```
https://api.dataforseo.com/v3/
```

All requests use HTTP Basic Auth:
```bash
curl -u "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" \
  -H "Content-Type: application/json" \
  https://api.dataforseo.com/v3/...
```

## Common Tasks

### 1. Keyword Search Volume (Google Ads)

POST /v3/keywords_data/google_ads/search_volume/live
Body: [{"keywords":["TARGET_KEYWORD"],"location_code":2840,"language_code":"en"}]
(location_code 2840 = United States)

### 2. SERP Results (Google Organic)

POST /v3/serp/google/organic/live/advanced
Body: [{"keyword":"TARGET_KEYWORD","location_code":2840,"language_code":"en","depth":10}]

### 3. Keyword Ideas / Related Keywords

POST /v3/keywords_data/google_ads/keywords_for_keywords/live
Body: [{"keywords":["SEED_KEYWORD"],"location_code":2840,"language_code":"en"}]

### 4. Backlink Summary for a Domain

POST /v3/backlinks/summary/live
Body: [{"target":"example.com","limit":10}]

### 5. Domain Rank Overview (organic traffic estimate)

POST /v3/dataforseo_labs/google/domain_rank_overview/live
Body: [{"target":"example.com","location_code":2840,"language_code":"en"}]

### 6. Check Account Balance / Usage

GET /v3/appendix/user_data

## Workflow

1. Identify which DataForSEO endpoint fits the request.
2. Run the API call using the env var credentials.
3. Parse the JSON response — data is inside tasks[0].result
4. Present data in a clean table or summary.
5. If the user asks for a CSV or export, format accordingly.

## Setup (for humans)

Add to your shell profile or agent environment:

  export DATAFORSEO_LOGIN="your-email@example.com"
  export DATAFORSEO_PASSWORD="your-api-password"

Or add to the Paperclip agent adapter config.

## Notes

- DataForSEO charges per API call. Use /live endpoints for instant results.
- Rate limits: ~2,000 requests/minute on paid plans.
- All responses: { status_code, tasks: [{ id, status_code, result: [...] }] }
