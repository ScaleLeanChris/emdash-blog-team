# Onboarding Checklist

Run this on your first heartbeat. Create each item as a Paperclip issue via the API so the team has work to do.

## Step 0: Create the goal and project

Before creating issues, set up the goal and project they belong to:

```
POST /api/companies/{companyId}/goals
{
  "title": "Launch and operate the emdash website",
  "description": "Get the site fully operational with a defined editorial mission, taxonomy structure, SEO foundation, and regular publishing cadence.",
  "level": "company",
  "status": "active"
}
→ Save the goalId from the response

POST /api/companies/{companyId}/projects
{
  "name": "Website Setup",
  "description": "Initial setup and onboarding — connect to the CMS, define editorial direction, create taxonomy, publish first content, and configure distribution.",
  "status": "planned",
  "goalIds": ["{goalId}"]
}
→ Save the projectId from the response
```

## Step 1: Connect to emdash (assign to yourself)

```
POST /api/companies/{companyId}/issues
{
  "title": "Connect to emdash instance and verify API access",
  "description": "Verify EMDASH_URL and EMDASH_API_TOKEN env vars are configured. Test with GET /_emdash/api/auth/me. Confirm token scopes. Run schema_get_collection to discover collections and field names. Document the schema for the team.",
  "status": "todo",
  "priority": "critical",
  "assigneeAgentId": "{your agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

**How to verify:** Call `GET $EMDASH_URL/_emdash/api/auth/me` with the Bearer token. You should see your user info with role 40+. Then call `schema_get_collection({ slug: "posts", includeFields: true })` to get field names.

## Step 2: Set editorial mission (assign to yourself)

```
POST /api/companies/{companyId}/issues
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

**This is the most important early decision.** Everything downstream depends on it.

## Step 3: Set up taxonomy (assign to Website Manager)

```
POST /api/companies/{companyId}/issues
{
  "title": "Set up taxonomy structure",
  "description": "Based on the content pillars, create categories (hierarchical) and tags (flat) using the taxonomy API. Taxonomy names are singular: 'category', 'tag'. Set up the primary navigation menu.",
  "status": "todo",
  "priority": "high",
  "assigneeAgentId": "{website-manager agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

## Step 4: Write first post (assign to Content Writer)

```
POST /api/companies/{companyId}/issues
{
  "title": "Write and publish first post",
  "description": "Check the collection schema for field names first (schema_get_collection). Write in Portable Text format. Include title, body, excerpt. Submit as draft for review, then publish. Note: publish endpoint requires empty JSON body {}.",
  "status": "todo",
  "priority": "high",
  "assigneeAgentId": "{content-writer agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

## Step 5: Configure SEO (assign to SEO Specialist)

```
POST /api/companies/{companyId}/issues
{
  "title": "Configure SEO foundation",
  "description": "First: enable SEO on the posts collection (PUT /schema/collections/posts with hasSeo: true — requires ADMIN). Then verify sitemap, check for 404s (GET /redirects/404s/summary), and set meta title + description on the first post.",
  "status": "todo",
  "priority": "medium",
  "assigneeAgentId": "{seo-specialist agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

## Step 6: Set up distribution (assign to Distribution Manager)

```
POST /api/companies/{companyId}/issues
{
  "title": "Set up content distribution channels",
  "description": "Identify available distribution channels (social media, newsletters, messaging). Configure channels and create templates for post-publish promotion. Distribute the first published post.",
  "status": "todo",
  "priority": "medium",
  "assigneeAgentId": "{distribution-manager agent id}",
  "projectId": "{projectId}",
  "goalId": "{goalId}"
}
```

## After onboarding

Once all 6 tasks are complete, the site is operational. Shift to ongoing operations:
- Website Manager maintains the content calendar
- Content Writer produces posts on cadence
- SEO Specialist monitors 404s and optimizes
- Distribution Manager promotes each published post
- You review performance and adjust strategy
