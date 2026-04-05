# Onboarding Checklist

Run this on your first heartbeat when no published content exists. Use the Paperclip skill to create the goal, project, and issues. All API calls go through the Paperclip control plane — use the `PAPERCLIP_API_URL`, `PAPERCLIP_API_KEY`, and `PAPERCLIP_RUN_ID` env vars that are auto-injected.

Always include the run ID header on mutations: `X-Paperclip-Run-Id: $PAPERCLIP_RUN_ID`

## Step 0: Create the goal and project

First, get your identity and company ID:

```
GET /api/agents/me
→ Save your agentId and companyId
```

Create the company goal:

```
POST /api/companies/{companyId}/goals
Headers: X-Paperclip-Run-Id: $PAPERCLIP_RUN_ID
{
  "title": "Launch and operate the emdash website",
  "description": "Get the site fully operational with a defined editorial mission, taxonomy structure, SEO foundation, and regular publishing cadence.",
  "level": "company",
  "status": "active"
}
→ Save the goalId from the response
```

Create the project:

```
POST /api/companies/{companyId}/projects
Headers: X-Paperclip-Run-Id: $PAPERCLIP_RUN_ID
{
  "name": "Website Setup",
  "description": "Initial setup and onboarding — connect to the CMS, define editorial direction, create taxonomy, publish first content, and configure distribution.",
  "status": "planned",
  "goalIds": ["{goalId}"]
}
→ Save the projectId from the response
```

## Step 1: Get agent IDs for task assignment

```
GET /api/companies/{companyId}/agents
```

Find the agent IDs for: CEO (yourself), Website Manager, Content Writer, SEO Specialist, Distribution Manager. Match by name.

## Step 2: Create onboarding issues

Create each issue via `POST /api/companies/{companyId}/issues` with `X-Paperclip-Run-Id` header. Set `projectId` and `goalId` on each.

### Issue 1: Connect to emdash (assign to yourself, priority: critical)

```json
{
  "title": "Connect to emdash instance and verify API access",
  "description": "Verify EMDASH_URL and EMDASH_API_TOKEN env vars are configured. Test by calling the emdash MCP tool content_list or the REST endpoint GET $EMDASH_URL/_emdash/api/auth/me. Confirm token scopes cover content and media. Run schema_get_collection for each collection to discover field names. Document the schema for the team.",
  "status": "todo",
  "priority": "critical",
  "assigneeAgentId": "{your agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

### Issue 2: Set editorial mission (assign to yourself, priority: high)

```json
{
  "title": "Set editorial mission and define content pillars",
  "description": "Define who the website serves, what value it provides, and what success looks like. Establish 3-5 content pillars (core topics). Document voice and tone guidelines.",
  "status": "todo",
  "priority": "high",
  "assigneeAgentId": "{your agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

### Issue 3: Set up taxonomy (assign to Website Manager, priority: high)

```json
{
  "title": "Set up taxonomy structure",
  "description": "Based on the content pillars from the CEO, create categories and tags using the emdash MCP tools (taxonomy_create_term). Taxonomy names are singular: 'category', 'tag'. Set up the primary navigation menu via the menu MCP tools or REST API.",
  "status": "todo",
  "priority": "high",
  "assigneeAgentId": "{website-manager agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

### Issue 4: Write first post (assign to Content Writer, priority: high)

```json
{
  "title": "Write and publish first post",
  "description": "Use schema_get_collection to check field names first. Write in Portable Text format (JSON blocks, not HTML). Use the content_create MCP tool to create a draft. Include title, body content, and excerpt. Submit as draft for review. After review, use content_publish to make it live (requires empty JSON body {} via REST).",
  "status": "todo",
  "priority": "high",
  "assigneeAgentId": "{content-writer agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

### Issue 5: Configure SEO (assign to SEO Specialist, priority: medium)

```json
{
  "title": "Configure SEO foundation",
  "description": "First: enable SEO on the posts collection via REST API (PUT /_emdash/api/schema/collections/posts with {hasSeo: true} — requires ADMIN token). Then verify sitemap works. Check for 404s via GET /_emdash/api/redirects/404s/summary. Set meta title and description on the first post using content_update with the seo field.",
  "status": "todo",
  "priority": "medium",
  "assigneeAgentId": "{seo-specialist agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

### Issue 6: Set up distribution (assign to Distribution Manager, priority: medium)

```json
{
  "title": "Set up content distribution channels",
  "description": "Identify what social/messaging tools are available in the agent environment. Configure channels and create templates for post-publish promotion. Use content_get to fetch published posts and create platform-specific promotional content.",
  "status": "todo",
  "priority": "medium",
  "assigneeAgentId": "{distribution-manager agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

## After creating all issues

1. Check out Issue 1 (emdash connection) and complete it yourself
2. Then check out Issue 2 (editorial mission) and complete it
3. Once you've set the mission, the other agents can pick up their assigned issues on their next heartbeats

## After onboarding

Once all 6 issues are done, shift to ongoing operations:
- Website Manager maintains the content calendar
- Content Writer produces posts on cadence
- SEO Specialist monitors 404s and optimizes
- Distribution Manager promotes each published post
- You review performance and adjust strategy
